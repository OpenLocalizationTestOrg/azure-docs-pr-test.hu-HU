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
# <a name="configure-ssl-connectivity-in-your-application-toosecurely-connect-tooazure-database-for-mysql"></a><span data-ttu-id="11021-103">Az SSL konfigurálása az alkalmazás toosecurely való csatlakozással tooAzure adatbázis kapcsolati MySQL</span><span class="sxs-lookup"><span data-stu-id="11021-103">Configure SSL connectivity in your application toosecurely connect tooAzure Database for MySQL</span></span>
<span data-ttu-id="11021-104">MySQL az Azure-adatbázishoz való csatlakozást az Azure-adatbázis MySQL tooclient olyan kiszolgálóalkalmazások esetén használja a Secure Sockets Layer (SSL) támogatja.</span><span class="sxs-lookup"><span data-stu-id="11021-104">Azure Database for MySQL supports connecting your Azure Database for MySQL server tooclient applications using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="11021-105">Az adatbázis-kiszolgáló és az ügyfél alkalmazások közötti SSL-kapcsolatok kényszerítése megvédi a "hello középső man" támadások ellen hello adatfolyam hello kiszolgáló és az alkalmazás közötti titkosításával.</span><span class="sxs-lookup"><span data-stu-id="11021-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in hello middle" attacks by encrypting hello data stream between hello server and your application.</span></span>

## <a name="step-1-obtain-ssl-certificate"></a><span data-ttu-id="11021-106">1. lépés: SSL-tanúsítvány beszerzése</span><span class="sxs-lookup"><span data-stu-id="11021-106">Step 1: Obtain SSL Certificate</span></span>
<span data-ttu-id="11021-107">Töltse le a hello tanúsítvány szükséges toocommunicate az Azure-adatbázissal a MySQL-kiszolgáló SSL-en keresztül [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) és hello tanúsítvány fájl tooyour helyi mentése meghajtó (ebben az oktatóanyagban a használtuk c:\ssl).</span><span class="sxs-lookup"><span data-stu-id="11021-107">Download hello certificate needed toocommunicate over SSL with your Azure Database for MySQL server from [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) and save hello certificate file tooyour local drive (with this tutorial, we used c:\ssl).</span></span>
<span data-ttu-id="11021-108">**A Microsoft Internet Explorer és a Microsoft Edge:** hello letöltés befejezése után nevezze át a hello tanúsítvány tooBaltimoreCyberTrustRoot.crt.pem.</span><span class="sxs-lookup"><span data-stu-id="11021-108">**For Microsoft Internet Explorer and Microsoft Edge:** After hello download has completed, rename hello certificate tooBaltimoreCyberTrustRoot.crt.pem.</span></span>

## <a name="step-2-bind-ssl"></a><span data-ttu-id="11021-109">2. lépés: A kötés SSL</span><span class="sxs-lookup"><span data-stu-id="11021-109">Step 2: Bind SSL</span></span>
### <a name="connecting-tooserver-using-hello-mysql-workbench-over-ssl"></a><span data-ttu-id="11021-110">Hello MySQL munkaterület használata az SSL-en keresztül csatlakozó tooserver</span><span class="sxs-lookup"><span data-stu-id="11021-110">Connecting tooserver using hello MySQL Workbench over SSL</span></span>
<span data-ttu-id="11021-111">Konfigurálja a MySQL munkaterület tooconnect biztonságos SSL-en keresztül.</span><span class="sxs-lookup"><span data-stu-id="11021-111">Configure MySQL Workbench tooconnect securely over SSL.</span></span> <span data-ttu-id="11021-112">Keresse meg a toohello **SSL** hello MySQL munkaterület az új kapcsolat beállítása párbeszéd hello lapján.</span><span class="sxs-lookup"><span data-stu-id="11021-112">Navigate toohello **SSL** tab in hello MySQL Workbench on hello Setup New Connection dialogue.</span></span> <span data-ttu-id="11021-113">Adja meg a fájl helye hello hello **BaltimoreCyberTrustRoot.crt.pem** a hello **SSL-hitelesítésszolgáltató fájl:** mező.</span><span class="sxs-lookup"><span data-stu-id="11021-113">Enter hello file location of hello **BaltimoreCyberTrustRoot.crt.pem** in hello **SSL CA File:** field.</span></span>
<span data-ttu-id="11021-114">![testre szabott csempe mentése](./media/howto-configure-ssl/mysql-workbench-ssl.png)</span><span class="sxs-lookup"><span data-stu-id="11021-114">![save customized tile](./media/howto-configure-ssl/mysql-workbench-ssl.png)</span></span>

### <a name="connecting-tooserver-using-hello-mysql-cli-over-ssl"></a><span data-ttu-id="11021-115">Hello MySQL CLI használata SSL-en keresztül csatlakozó tooserver</span><span class="sxs-lookup"><span data-stu-id="11021-115">Connecting tooserver using hello MySQL CLI over SSL</span></span>
<span data-ttu-id="11021-116">A következő parancs hello hello MySQL parancssori felület segítségével hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="11021-116">Using hello MySQL command-line interface, execute hello following command:</span></span>
```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -u Username@mysqlserver4demo -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a><span data-ttu-id="11021-117">3. lépés: SSL-kapcsolatok az Azure-ban kényszerítése</span><span class="sxs-lookup"><span data-stu-id="11021-117">Step 3:  Enforcing SSL connections in Azure</span></span> 
### <a name="using-azure-portal"></a><span data-ttu-id="11021-118">Az Azure Portal használata</span><span class="sxs-lookup"><span data-stu-id="11021-118">Using Azure portal</span></span>
<span data-ttu-id="11021-119">Hello Azure-portál használatával keresse fel a MySQL-kiszolgálót, majd kattintson az Azure Database **kapcsolatbiztonsági**.</span><span class="sxs-lookup"><span data-stu-id="11021-119">Using hello Azure portal, visit your Azure Database for MySQL server and click **Connection security**.</span></span> <span data-ttu-id="11021-120">Hello váltása gomb tooenable használja, vagy tiltsa le a hello **kényszerítése SSL-kapcsolat** beállítást.</span><span class="sxs-lookup"><span data-stu-id="11021-120">Use hello toggle button tooenable or disable hello **Enforce SSL connection** setting.</span></span> <span data-ttu-id="11021-121">Ezután kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="11021-121">Then click **Save**.</span></span> <span data-ttu-id="11021-122">A Microsoft azt javasolja, tooalways engedélyezése **kényszerítése SSL-kapcsolat** vonatkozó fokozott biztonsági beállításait.</span><span class="sxs-lookup"><span data-stu-id="11021-122">Microsoft recommends tooalways enable **Enforce SSL connection** setting for enhanced security.</span></span>
<span data-ttu-id="11021-123">![ssl engedélyezése](./media/howto-configure-ssl/enable-ssl.png)</span><span class="sxs-lookup"><span data-stu-id="11021-123">![enable-ssl](./media/howto-configure-ssl/enable-ssl.png)</span></span>

### <a name="using-azure-cli"></a><span data-ttu-id="11021-124">Az Azure parancssori felület használata</span><span class="sxs-lookup"><span data-stu-id="11021-124">Using Azure CLI</span></span>
<span data-ttu-id="11021-125">Engedélyezheti vagy letilthatja a hello **ssl-kényszerítési** paraméter, illetve Azure CLI engedélyezve vagy letiltva értékei alapján.</span><span class="sxs-lookup"><span data-stu-id="11021-125">You can enable or disable hello **ssl-enforcement** parameter using Enabled or Disabled values respectively in Azure CLI.</span></span>
```azurecli-interactive
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```

## <a name="step-4-verify-ssl-connection"></a><span data-ttu-id="11021-126">4. lépés: SSL-kapcsolat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="11021-126">Step 4: Verify SSL Connection</span></span>
<span data-ttu-id="11021-127">Hello mysql végrehajtása **állapot** parancs tooverify, hogy csatlakozott-e tooyour MySQL kiszolgálóval SSL használatával:</span><span class="sxs-lookup"><span data-stu-id="11021-127">Execute hello mysql **status** command tooverify that you have connected tooyour MySQL server using SSL:</span></span>
```dos
mysql> status
```
<span data-ttu-id="11021-128">Győződjön meg róla, hello kapcsolat titkosított hello kimeneti megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="11021-128">Confirm hello connection is encrypted by reviewing hello output.</span></span> <span data-ttu-id="11021-129">Kell megjelennie: **SSL: titkosító használatban AES256-SHA**</span><span class="sxs-lookup"><span data-stu-id="11021-129">It should show:  **SSL: Cipher in use is AES256-SHA**</span></span> 

## <a name="sample-code"></a><span data-ttu-id="11021-130">Mintakód</span><span class="sxs-lookup"><span data-stu-id="11021-130">Sample code</span></span>
### <a name="php"></a><span data-ttu-id="11021-131">PHP</span><span class="sxs-lookup"><span data-stu-id="11021-131">PHP</span></span>
```
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'myserver4demo.mysql.database.azure.com', 'myadmin@myserver4demo', 'yourpassword', 'quickstartdb', 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
}
```
### <a name="python"></a><span data-ttu-id="11021-132">Python</span><span class="sxs-lookup"><span data-stu-id="11021-132">Python</span></span>
```
try:
conn = mysql.connector.connect(user='myadmin@myserver4demo',password='yourpassword',database='quickstartdb',host='myserver4demo.mysql.database.azure.com',ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
 print(err)
```

## <a name="next-steps"></a><span data-ttu-id="11021-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="11021-133">Next steps</span></span>
<span data-ttu-id="11021-134">Tekintse át a következő különböző alkalmazás kapcsolati lehetőségek [adatkapcsolattárak MySQL az Azure-adatbázis](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="11021-134">Review various application connectivity options following [Connection libraries for Azure Database for MySQL](concepts-connection-libraries.md)</span></span>
