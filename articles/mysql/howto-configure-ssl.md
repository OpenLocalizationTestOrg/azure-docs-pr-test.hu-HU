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
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mysql"></a><span data-ttu-id="24d63-103">Az alkalmazásokhoz való biztonságos kapcsolódás Azure-adatbázis a MySQL SSL-kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="24d63-103">Configure SSL connectivity in your application to securely connect to Azure Database for MySQL</span></span>
<span data-ttu-id="24d63-104">Azure MySQL-adatbázis támogatja az Azure-adatbázis MySQL-kiszolgáló csatlakozik a Secure Sockets Layer (SSL) használó ügyfélalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="24d63-104">Azure Database for MySQL supports connecting your Azure Database for MySQL server to client applications using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="24d63-105">Az adatbázis-kiszolgáló és az ügyfél alkalmazások közötti SSL-kapcsolatok kényszerítése segít lánctámadások elleni védelem érdekében "man a középső" az adatfolyamot a kiszolgáló és az alkalmazás közötti titkosításával.</span><span class="sxs-lookup"><span data-stu-id="24d63-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in the middle" attacks by encrypting the data stream between the server and your application.</span></span>

## <a name="step-1-obtain-ssl-certificate"></a><span data-ttu-id="24d63-106">1. lépés: SSL-tanúsítvány beszerzése</span><span class="sxs-lookup"><span data-stu-id="24d63-106">Step 1: Obtain SSL Certificate</span></span>
<span data-ttu-id="24d63-107">Töltse le a tanúsítványt, az Azure-adatbázis a MySQL-kiszolgáló az SSL protokollt használó kommunikációra szükséges [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) és mentette a tanúsítványfájlt (a helyi meghajtójára Ebben az oktatóanyagban használtuk c:\ssl).</span><span class="sxs-lookup"><span data-stu-id="24d63-107">Download the certificate needed to communicate over SSL with your Azure Database for MySQL server from [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) and save the certificate file to your local drive (with this tutorial, we used c:\ssl).</span></span>
<span data-ttu-id="24d63-108">**A Microsoft Internet Explorer és a Microsoft Edge:** a letöltés befejezése után nevezze át a tanúsítvány BaltimoreCyberTrustRoot.crt.pem.</span><span class="sxs-lookup"><span data-stu-id="24d63-108">**For Microsoft Internet Explorer and Microsoft Edge:** After the download has completed, rename the certificate to BaltimoreCyberTrustRoot.crt.pem.</span></span>

## <a name="step-2-bind-ssl"></a><span data-ttu-id="24d63-109">2. lépés: A kötés SSL</span><span class="sxs-lookup"><span data-stu-id="24d63-109">Step 2: Bind SSL</span></span>
### <a name="connecting-to-server-using-the-mysql-workbench-over-ssl"></a><span data-ttu-id="24d63-110">Kapcsolódás a kiszolgálóhoz a MySQL munkaterület használata az SSL-en keresztül</span><span class="sxs-lookup"><span data-stu-id="24d63-110">Connecting to server using the MySQL Workbench over SSL</span></span>
<span data-ttu-id="24d63-111">Konfigurálja a MySQL munkaterület biztonságos SSL Csatornán keresztül csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="24d63-111">Configure MySQL Workbench to connect securely over SSL.</span></span> <span data-ttu-id="24d63-112">Keresse meg a **SSL** a MySQL munkaterület az új kapcsolat beállítása párbeszéd fülre.</span><span class="sxs-lookup"><span data-stu-id="24d63-112">Navigate to the **SSL** tab in the MySQL Workbench on the Setup New Connection dialogue.</span></span> <span data-ttu-id="24d63-113">Adja meg a fájl helyét a **BaltimoreCyberTrustRoot.crt.pem** a a **SSL-hitelesítésszolgáltató fájl:** mező.</span><span class="sxs-lookup"><span data-stu-id="24d63-113">Enter the file location of the **BaltimoreCyberTrustRoot.crt.pem** in the **SSL CA File:** field.</span></span>
<span data-ttu-id="24d63-114">![testre szabott csempe mentése](./media/howto-configure-ssl/mysql-workbench-ssl.png)</span><span class="sxs-lookup"><span data-stu-id="24d63-114">![save customized tile](./media/howto-configure-ssl/mysql-workbench-ssl.png)</span></span>

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a><span data-ttu-id="24d63-115">Kapcsolódás a kiszolgálóhoz SSL-en keresztül a MySQL parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="24d63-115">Connecting to server using the MySQL CLI over SSL</span></span>
<span data-ttu-id="24d63-116">A MySQL parancssori felületén, hajtsa végre a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="24d63-116">Using the MySQL command-line interface, execute the following command:</span></span>
```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -u Username@mysqlserver4demo -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a><span data-ttu-id="24d63-117">3. lépés: SSL-kapcsolatok az Azure-ban kényszerítése</span><span class="sxs-lookup"><span data-stu-id="24d63-117">Step 3:  Enforcing SSL connections in Azure</span></span> 
### <a name="using-azure-portal"></a><span data-ttu-id="24d63-118">Az Azure Portal használata</span><span class="sxs-lookup"><span data-stu-id="24d63-118">Using Azure portal</span></span>
<span data-ttu-id="24d63-119">Az Azure-portált használja, látogasson el a MySQL-kiszolgálót, majd kattintson az Azure Database **kapcsolatbiztonsági**.</span><span class="sxs-lookup"><span data-stu-id="24d63-119">Using the Azure portal, visit your Azure Database for MySQL server and click **Connection security**.</span></span> <span data-ttu-id="24d63-120">A váltógomb segítségével engedélyezheti vagy tilthatja le a **kényszerítése SSL-kapcsolat** beállítást.</span><span class="sxs-lookup"><span data-stu-id="24d63-120">Use the toggle button to enable or disable the **Enforce SSL connection** setting.</span></span> <span data-ttu-id="24d63-121">Ezután kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="24d63-121">Then click **Save**.</span></span> <span data-ttu-id="24d63-122">A Microsoft azt javasolja, hogy mindig engedélyezése **kényszerítése SSL-kapcsolat** vonatkozó fokozott biztonsági beállításait.</span><span class="sxs-lookup"><span data-stu-id="24d63-122">Microsoft recommends to always enable **Enforce SSL connection** setting for enhanced security.</span></span>
<span data-ttu-id="24d63-123">![ssl engedélyezése](./media/howto-configure-ssl/enable-ssl.png)</span><span class="sxs-lookup"><span data-stu-id="24d63-123">![enable-ssl](./media/howto-configure-ssl/enable-ssl.png)</span></span>

### <a name="using-azure-cli"></a><span data-ttu-id="24d63-124">Az Azure parancssori felület használata</span><span class="sxs-lookup"><span data-stu-id="24d63-124">Using Azure CLI</span></span>
<span data-ttu-id="24d63-125">Engedélyezheti vagy letilthatja a **ssl-kényszerítési** paraméter, illetve Azure CLI engedélyezve vagy letiltva értékei alapján.</span><span class="sxs-lookup"><span data-stu-id="24d63-125">You can enable or disable the **ssl-enforcement** parameter using Enabled or Disabled values respectively in Azure CLI.</span></span>
```azurecli-interactive
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```

## <a name="step-4-verify-ssl-connection"></a><span data-ttu-id="24d63-126">4. lépés: SSL-kapcsolat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="24d63-126">Step 4: Verify SSL Connection</span></span>
<span data-ttu-id="24d63-127">A mysql végrehajtása **állapot** parancs futtatásával ellenőrizze, hogy rendelkezik-e csatlakozik a MySQL-kiszolgálóval SSL használatával:</span><span class="sxs-lookup"><span data-stu-id="24d63-127">Execute the mysql **status** command to verify that you have connected to your MySQL server using SSL:</span></span>
```dos
mysql> status
```
<span data-ttu-id="24d63-128">Ellenőrizze, hogy a kapcsolat a kimeneti megtekintésével titkosított.</span><span class="sxs-lookup"><span data-stu-id="24d63-128">Confirm the connection is encrypted by reviewing the output.</span></span> <span data-ttu-id="24d63-129">Kell megjelennie: **SSL: titkosító használatban AES256-SHA**</span><span class="sxs-lookup"><span data-stu-id="24d63-129">It should show:  **SSL: Cipher in use is AES256-SHA**</span></span> 

## <a name="sample-code"></a><span data-ttu-id="24d63-130">Mintakód</span><span class="sxs-lookup"><span data-stu-id="24d63-130">Sample code</span></span>
### <a name="php"></a><span data-ttu-id="24d63-131">PHP</span><span class="sxs-lookup"><span data-stu-id="24d63-131">PHP</span></span>
```
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'myserver4demo.mysql.database.azure.com', 'myadmin@myserver4demo', 'yourpassword', 'quickstartdb', 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="python"></a><span data-ttu-id="24d63-132">Python</span><span class="sxs-lookup"><span data-stu-id="24d63-132">Python</span></span>
```
try:
conn = mysql.connector.connect(user='myadmin@myserver4demo',password='yourpassword',database='quickstartdb',host='myserver4demo.mysql.database.azure.com',ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
 print(err)
```

## <a name="next-steps"></a><span data-ttu-id="24d63-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="24d63-133">Next steps</span></span>
<span data-ttu-id="24d63-134">Tekintse át a következő különböző alkalmazás kapcsolati lehetőségek [adatkapcsolattárak MySQL az Azure-adatbázis](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="24d63-134">Review various application connectivity options following [Connection libraries for Azure Database for MySQL](concepts-connection-libraries.md)</span></span>
