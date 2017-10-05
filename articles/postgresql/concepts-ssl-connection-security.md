---
title: "Állítsa be a SSL-kapcsolatot az Azure Database PostgreSQL |} Microsoft Docs"
description: "Utasításokat és az információkat PostgreSQL és a társított alkalmazások megfelelően az SSL-kapcsolat használata Azure-adatbázis konfigurálása."
services: postgresql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: postgresql
ms.custom: 
ms.topic: article
ms.date: 05/15/2017
ms.openlocfilehash: 685aa4c2f75b7c3260ca737f7c786157480b2d90
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a><span data-ttu-id="36b75-103">SSL-kapcsolatot PostgreSQL Azure-adatbázis konfigurálása</span><span class="sxs-lookup"><span data-stu-id="36b75-103">Configure SSL connectivity in Azure Database for PostgreSQL</span></span>
<span data-ttu-id="36b75-104">Azure-adatbázis PostgreSQL inkább csatlakozás az ügyfél alkalmazásait, és a Secure Sockets Layer (SSL) használatával PostgreSQL-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="36b75-104">Azure Database for PostgreSQL prefers connecting your client applications to the PostgreSQL service using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="36b75-105">Az adatbázis-kiszolgáló és az ügyfél alkalmazások közötti SSL-kapcsolatok kényszerítése segít lánctámadások elleni védelem érdekében "man a középső" az adatfolyamot a kiszolgáló és az alkalmazás közötti titkosításával.</span><span class="sxs-lookup"><span data-stu-id="36b75-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in the middle" attacks by encrypting the data stream between the server and your application.</span></span>

<span data-ttu-id="36b75-106">Alapértelmezés szerint a PostgreSQL-adatbázis szolgáltatás SSL-kapcsolat megkövetelése van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="36b75-106">By default, the PostgreSQL database service is configured to require SSL connection.</span></span> <span data-ttu-id="36b75-107">Másik lehetőségként letilthatja az adatbázis-szolgáltatás szeretne csatlakozni, ha az ügyfélalkalmazást nem támogatja az SSL-kapcsolatot az SSL megkövetelése.</span><span class="sxs-lookup"><span data-stu-id="36b75-107">Optionally, you can disable requiring SSL for connecting to your database service if your client application does not support SSL connectivity.</span></span> 

## <a name="enforcing-ssl-connections"></a><span data-ttu-id="36b75-108">SSL-kapcsolatok kényszerítése</span><span class="sxs-lookup"><span data-stu-id="36b75-108">Enforcing SSL connections</span></span>
<span data-ttu-id="36b75-109">Az összes Azure Database az Azure portál és a parancssori felületen keresztül szerezhetők PostgreSQL-kiszolgálók esetén az SSL-kapcsolatok kényszerítési alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="36b75-109">For all Azure Database for PostgreSQL servers provisioned through the Azure portal and CLI, enforcement of SSL connections is enabled by default.</span></span> 

<span data-ttu-id="36b75-110">Hasonlóképpen a "Kapcsolati karakterláncok" beállításokat a kiszolgálón az Azure-portálon előre definiált kapcsolati karakterláncok közös nyelvek SSL használatával az adatbázis-kiszolgálóhoz való csatlakozáshoz szükséges paraméterek közé tartoznak.</span><span class="sxs-lookup"><span data-stu-id="36b75-110">Likewise, connection strings that are pre-defined in the "Connection Strings" settings under your server in the Azure portal include the required parameters for common languages to connect to your database server using SSL.</span></span> <span data-ttu-id="36b75-111">Az SSL-paraméter attól függően változik, az összekötő, például "ssl = true" vagy "sslmode = szükséges" vagy "sslmode = szükséges" és egyéb változatok.</span><span class="sxs-lookup"><span data-stu-id="36b75-111">The SSL parameter varies based on the connector, for example "ssl=true" or "sslmode=require" or "sslmode=required" and other variations.</span></span>

## <a name="configure-enforcement-of-ssl"></a><span data-ttu-id="36b75-112">Az SSL kényszerítésének</span><span class="sxs-lookup"><span data-stu-id="36b75-112">Configure Enforcement of SSL</span></span>
<span data-ttu-id="36b75-113">Igény szerint letilthatja végrehajtó SSL-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="36b75-113">You can optionally disable enforcing SSL connectivity.</span></span> <span data-ttu-id="36b75-114">A Microsoft Azure azt javasolja, hogy mindig engedélyezése **kényszerítése SSL-kapcsolat** vonatkozó fokozott biztonsági beállításait.</span><span class="sxs-lookup"><span data-stu-id="36b75-114">Microsoft Azure recommends to always enable **Enforce SSL connection** setting for enhanced security.</span></span>

### <a name="using-the-azure-portal"></a><span data-ttu-id="36b75-115">Az Azure Portal használata</span><span class="sxs-lookup"><span data-stu-id="36b75-115">Using the Azure portal</span></span>
<span data-ttu-id="36b75-116">Keresse fel az Azure-adatbázis PostgreSQL-kiszolgáló, és kattintson a **kapcsolatbiztonsági**.</span><span class="sxs-lookup"><span data-stu-id="36b75-116">Visit your Azure Database for PostgreSQL server and click **Connection security**.</span></span> <span data-ttu-id="36b75-117">A váltógomb segítségével engedélyezheti vagy tilthatja le a **kényszerítése SSL-kapcsolat** beállítást.</span><span class="sxs-lookup"><span data-stu-id="36b75-117">Use the toggle button to enable or disable the **Enforce SSL connection** setting.</span></span> <span data-ttu-id="36b75-118">Ezután kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="36b75-118">Then click **Save**.</span></span> 

![Kapcsolat biztonsági - Disable SSL kényszerítése](./media/concepts-ssl-connection-security/1-disable-ssl.png)

<span data-ttu-id="36b75-120">Megtekintésével ellenőrizheti a beállítás a **áttekintése** lapot, melyen megtekintheti a **SSL kényszerítése állapot** mutató.</span><span class="sxs-lookup"><span data-stu-id="36b75-120">You can confirm the setting by viewing the **Overview** page to see the **SSL enforce status** indicator.</span></span>

### <a name="using-azure-cli"></a><span data-ttu-id="36b75-121">Az Azure parancssori felület használata</span><span class="sxs-lookup"><span data-stu-id="36b75-121">Using Azure CLI</span></span>
<span data-ttu-id="36b75-122">Engedélyezheti vagy letilthatja a **ssl-kényszerítési** paraméter használatával `Enabled` vagy `Disabled` értékeket az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="36b75-122">You can enable or disable the **ssl-enforcement** parameter using `Enabled` or `Disabled` values respectively in Azure CLI.</span></span>

```azurecli
az postgres server update --resource-group myresourcegroup --name mypgserver-20170401 --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a><span data-ttu-id="36b75-123">Ellenőrizze az alkalmazás vagy a keretrendszer támogatja SSL-kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="36b75-123">Ensure your application or framework supports SSL connections</span></span>
<span data-ttu-id="36b75-124">Sok általános alkalmazás-keretrendszert használó PostgreSQL az adatbázis-szolgáltatásokhoz, például a Drupal és a Django, ne engedélyezze az SSL alapértelmezés szerint telepítése során.</span><span class="sxs-lookup"><span data-stu-id="36b75-124">Many common application frameworks that use PostgreSQL for database services, such as Drupal and Django, do not enable SSL by default during installation.</span></span> <span data-ttu-id="36b75-125">Engedélyezi az SSL-kapcsolatot kell elvégezni, a telepítés után, vagy a parancssori felület parancsait az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="36b75-125">Enabling SSL connectivity must be done after installation or through CLI commands specific to the application.</span></span> <span data-ttu-id="36b75-126">Ha a PostgreSQL-kiszolgáló SSL-kapcsolatok kényszerít, és az ahhoz kapcsolódó alkalmazás nincs megfelelően konfigurálva, az alkalmazás sikertelen lehet az adatbázis-kiszolgálóhoz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="36b75-126">If your PostgreSQL server is enforcing SSL connections and the associated application is not configured properly, the application may fail to connect to your database server.</span></span> <span data-ttu-id="36b75-127">Az alkalmazás dokumentációból megtudhatja, hogyan SSL-kapcsolatok engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="36b75-127">Consult your application's documentation to learn how to enable SSL connections.</span></span>


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a><span data-ttu-id="36b75-128">A tanúsítványellenőrzés az SSL-kapcsolatot igénylő alkalmazások</span><span class="sxs-lookup"><span data-stu-id="36b75-128">Applications that require certificate verification for SSL connectivity</span></span>
<span data-ttu-id="36b75-129">Bizonyos esetekben az alkalmazásoknak a megbízható tanúsítvány hitelesítésszolgáltatói (CA) tanúsítvány (.cer) kapcsolódó fájl biztonságosan által létrehozott helyi tanúsítványfájlt.</span><span class="sxs-lookup"><span data-stu-id="36b75-129">In some cases, applications require a local certificate file generated from a trusted Certificate Authority (CA) certificate file (.cer) to connect securely.</span></span> <span data-ttu-id="36b75-130">A következő lépések szerezze be a .cer fájlt, a tanúsítvány dekódolása, és kösse az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="36b75-130">See the following steps to obtain the .cer file, decode the certificate and bind it to your application.</span></span>

### <a name="download-the-certificate-file-from-the-certificate-authority-ca"></a><span data-ttu-id="36b75-131">Töltse le a tanúsítványt a tanúsítvány hitelesítésszolgáltatói (CA)</span><span class="sxs-lookup"><span data-stu-id="36b75-131">Download the certificate file from the Certificate Authority (CA)</span></span> 
<span data-ttu-id="36b75-132">Az Azure-adatbázissal SSL protokollt használó kommunikációra a PostgreSQL-kiszolgáló szükség a tanúsítvány [Itt](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt).</span><span class="sxs-lookup"><span data-stu-id="36b75-132">The certificate needed to communicate over SSL with your Azure Database for PostgreSQL server is located [here](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt).</span></span> <span data-ttu-id="36b75-133">Töltse le a fájlt helyileg.</span><span class="sxs-lookup"><span data-stu-id="36b75-133">Download the certificate file locally.</span></span>

### <a name="download-and-install-openssl-on-your-machine"></a><span data-ttu-id="36b75-134">Töltse le és telepítse a OpenSSL a számítógépen</span><span class="sxs-lookup"><span data-stu-id="36b75-134">Download and install OpenSSL on your machine</span></span> 
<span data-ttu-id="36b75-135">A tanúsítványfájl való biztonságos kapcsolódás az adatbázis-kiszolgáló az alkalmazáshoz szükséges dekódolni, telepítendő OpenSSL a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="36b75-135">To decode the certificate file needed for your application to connect securely to your database server, you need to install OpenSSL on your local computer.</span></span>

#### <a name="for-linux-os-x-or-unix"></a><span data-ttu-id="36b75-136">A Linux, OS X or Unix</span><span class="sxs-lookup"><span data-stu-id="36b75-136">For Linux, OS X, or Unix</span></span>
<span data-ttu-id="36b75-137">Az OpenSSL könyvtárakat közvetlenül a forráskód szerepelnek a [OpenSSL szoftver Foundation](http://www.openssl.org).</span><span class="sxs-lookup"><span data-stu-id="36b75-137">The OpenSSL libraries are provided in source code directly from the [OpenSSL Software Foundation](http://www.openssl.org).</span></span> <span data-ttu-id="36b75-138">Az alábbi utasítások alapján végigvezetik a Linux rendszerű számítógépen lévő OpenSSL telepítéséhez szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="36b75-138">The following instructions guide you through the steps necessary to install OpenSSL on your Linux PC.</span></span> <span data-ttu-id="36b75-139">Ebben a cikkben az ismert működését az Ubuntu 12.04 és magasabb parancsok.</span><span class="sxs-lookup"><span data-stu-id="36b75-139">This article uses commands known to work on Ubuntu 12.04 and higher.</span></span>

<span data-ttu-id="36b75-140">Nyisson meg egy terminál-munkamenetet, és OpenSSL telepítése</span><span class="sxs-lookup"><span data-stu-id="36b75-140">Open a terminal session and install OpenSSL</span></span>
```bash
wget http://www.openssl.org/source/openssl-1.1.0e.tar.gz
``` 
<span data-ttu-id="36b75-141">Csomagolja ki a fájlokat a letöltött csomag</span><span class="sxs-lookup"><span data-stu-id="36b75-141">Extract the files from the download package</span></span>
```bash
tar -xvzf openssl-1.1.0e.tar.gz
```
<span data-ttu-id="36b75-142">Adja meg a könyvtárat, ha a fájlok könyvtárban találhatók.</span><span class="sxs-lookup"><span data-stu-id="36b75-142">Enter the directory where the files were extracted.</span></span> <span data-ttu-id="36b75-143">Alapértelmezés szerint az alábbinak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="36b75-143">By default, it should be as follows.</span></span>

```bash
cd openssl-1.1.0e
```
<span data-ttu-id="36b75-144">OpenSSL konfigurálásához futtassa a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="36b75-144">Configure OpenSSL by executing the following command.</span></span> <span data-ttu-id="36b75-145">Ha szeretne egy mappában lévő fájlok /usr/local/openssl eltér, győződjön meg arról, módosíthatja a megfelelő műveletet.</span><span class="sxs-lookup"><span data-stu-id="36b75-145">If you want the files in a folder different than /usr/local/openssl, make sure to change the following as appropriate.</span></span>

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
<span data-ttu-id="36b75-146">Most, hogy OpenSSL megfelelően van konfigurálva, kell, hogy a tanúsítvány konvertálni.</span><span class="sxs-lookup"><span data-stu-id="36b75-146">Now that OpenSSL is configured properly, you need to compile it to convert your certificate.</span></span> <span data-ttu-id="36b75-147">Fordítása, futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="36b75-147">To compile, run the following command:</span></span>

```bash
make
```
<span data-ttu-id="36b75-148">Ha fordítása befejeződött, készen áll OpenSSL telepítse végrehajtható fájlként, a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="36b75-148">Once compiling is complete, you're ready to install OpenSSL as an executable by running the following command:</span></span>
```bash
make install
```
<span data-ttu-id="36b75-149">Győződjön meg arról, hogy sikeresen telepítette OpenSSL a rendszeren, futtassa a következő parancs és a jelölőnégyzet győződjön meg arról, hogy az azonos kimenethez kap.</span><span class="sxs-lookup"><span data-stu-id="36b75-149">To confirm that you've successfully installed OpenSSL on your system, run the following command and check to make sure you get the same output.</span></span>

```bash
/usr/local/openssl/bin/openssl version
```
<span data-ttu-id="36b75-150">Ha sikeres a következő üzenetet kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="36b75-150">If successful you should see the following message.</span></span>
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

#### <a name="for-windows"></a><span data-ttu-id="36b75-151">Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="36b75-151">For Windows</span></span>
<span data-ttu-id="36b75-152">OpenSSL telepítése Windows rendszerű megteheti a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="36b75-152">Installing OpenSSL on a Windows PC can be done in the following ways:</span></span>
1. <span data-ttu-id="36b75-153">**(Ajánlott)**  Funkcióval a beépített Bash a Windows a Windows 10-es és újabb verziók, OpenSSL alapértelmezés szerint telepítve van-e.</span><span class="sxs-lookup"><span data-stu-id="36b75-153">**(Recommended)** Using the built-in Bash for Windows functionality in Window 10 and above, OpenSSL is installed by default.</span></span> <span data-ttu-id="36b75-154">Windows 10 Bash a Windows-funkció engedélyezésével kapcsolatos útmutatást itt talál [Itt](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).</span><span class="sxs-lookup"><span data-stu-id="36b75-154">Instructions on how to enable Bash for Windows functionality in Windows 10 can be found [here](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).</span></span>
2. <span data-ttu-id="36b75-155">A letöltés a Win32/64 alkalmazás a Közösség által meghatározott.</span><span class="sxs-lookup"><span data-stu-id="36b75-155">Through downloading a Win32/64 application provided by the community.</span></span> <span data-ttu-id="36b75-156">Során a OpenSSL szoftver Foundation nem adja meg, vagy hagyja jóvá a megadott Windows Installer telepítők elérhető telepítők listáját tartalmazzák [Itt](https://wiki.openssl.org/index.php/Binaries)</span><span class="sxs-lookup"><span data-stu-id="36b75-156">While the OpenSSL Software Foundation does not provide or endorse any specific Windows installers, they provide a list of available installers [here](https://wiki.openssl.org/index.php/Binaries)</span></span>

### <a name="decode-your-certificate-file"></a><span data-ttu-id="36b75-157">A tanúsítványfájl dekódolása</span><span class="sxs-lookup"><span data-stu-id="36b75-157">Decode your certificate file</span></span>
<span data-ttu-id="36b75-158">A legfelső szintű hitelesítésszolgáltató letöltött fájl titkosított formátumban van.</span><span class="sxs-lookup"><span data-stu-id="36b75-158">The downloaded Root CA file is in encrypted format.</span></span> <span data-ttu-id="36b75-159">A tanúsítványfájl dekódolás OpenSSL használja.</span><span class="sxs-lookup"><span data-stu-id="36b75-159">Use OpenSSL to decode the certificate file.</span></span> <span data-ttu-id="36b75-160">Ehhez a OpenSSL parancsot:</span><span class="sxs-lookup"><span data-stu-id="36b75-160">To do so, run this OpenSSL command:</span></span>

```dos
OpenSSL>x509 -inform DER -in BaltimoreCyberTrustRoot.cer -text -out root.crt
```

### <a name="connecting-to-azure-database-for-postgresql-with-ssl-certificate-authentication"></a><span data-ttu-id="36b75-161">Azure-adatbázishoz szeretne csatlakozni a PostgreSQL SSL tanúsítványalapú hitelesítéssel ellátott</span><span class="sxs-lookup"><span data-stu-id="36b75-161">Connecting to Azure Database for PostgreSQL with SSL certificate authentication</span></span>
<span data-ttu-id="36b75-162">Most, hogy sikeres rendelkeznek dekódolni, a tanúsítványt, akkor is csatlakozhat az adatbázis-kiszolgáló biztonságos SSL-en keresztül.</span><span class="sxs-lookup"><span data-stu-id="36b75-162">Now that you have successfully decoded your certificate, you can now connect to your database server securely over SSL.</span></span> <span data-ttu-id="36b75-163">Ahhoz, hogy a kiszolgáló tanúsítványellenőrzést, a tanúsítványt kell helyezni a fájlt ~/.postgresql/root.crt az a felhasználó saját könyvtárához.</span><span class="sxs-lookup"><span data-stu-id="36b75-163">To allow server certificate verification, the certificate must be placed in the file ~/.postgresql/root.crt in the user's home directory.</span></span> <span data-ttu-id="36b75-164">(A Microsoft Windows a fájl neve % APPDATA%\postgresql\root.crt.).</span><span class="sxs-lookup"><span data-stu-id="36b75-164">(On Microsoft Windows the file is named %APPDATA%\postgresql\root.crt.).</span></span> <span data-ttu-id="36b75-165">A következő útmutatás PostgreSQL az Azure-adatbázishoz szeretne csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="36b75-165">The following provides instructions for connecting to Azure Database for PostgreSQL.</span></span>

> [!NOTE]
> <span data-ttu-id="36b75-166">Jelenleg egy ismert probléma használatakor "sslmode ellenőrizze teljes =" a kapcsolat a szolgáltatással, a kapcsolat nem tud a következő hiba miatt: _kiszolgálótanúsítvány "&lt;régió&gt;. control.database.windows.net" (és más neveket 7) nem egyezik meg a gazdagép neve "&lt;kiszolgálónév&gt;. postgres.database.azure.com"._</span><span class="sxs-lookup"><span data-stu-id="36b75-166">Currently, there is a known issue if you use "sslmode=verify-full" in your connection to the service, the connection will fail with the following error: _server certificate for "&lt;region&gt;.control.database.windows.net" (and 7 other names) does not match host name "&lt;servername&gt;.postgres.database.azure.com"._</span></span>
> <span data-ttu-id="36b75-167">Ha "sslmode ellenőrizze teljes =" van szükség, használja a kiszolgáló elnevezési konvenció  **&lt;kiszolgálónév&gt;. database.windows.net** a kapcsolati karakterlánc állomás neve.</span><span class="sxs-lookup"><span data-stu-id="36b75-167">If "sslmode=verify-full" is required, please use the server naming convention **&lt;servername&gt;.database.windows.net** as your connection string host name.</span></span> <span data-ttu-id="36b75-168">Távolítsa el ezt a korlátozást a jövőben tervezzük.</span><span class="sxs-lookup"><span data-stu-id="36b75-168">We plan to remove this limitation in the future.</span></span> <span data-ttu-id="36b75-169">Egyéb használó kapcsolatok [SSL módok](https://www.postgresql.org/docs/9.6/static/libpq-ssl.html#LIBPQ-SSL-SSLMODE-STATEMENTS) továbbra is használja az előnyben részesített elnevezési  **&lt;kiszolgálónév&gt;. postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="36b75-169">Connections using other [SSL modes](https://www.postgresql.org/docs/9.6/static/libpq-ssl.html#LIBPQ-SSL-SSLMODE-STATEMENTS) should continue to use the preferred host naming convention **&lt;servername&gt;.postgres.database.azure.com**.</span></span>

#### <a name="using-psql-command-line-utility"></a><span data-ttu-id="36b75-170">Psql parancssori segédprogrammal</span><span class="sxs-lookup"><span data-stu-id="36b75-170">Using psql command-line utility</span></span>
<span data-ttu-id="36b75-171">A következő példa bemutatja, hogyan csatlakozni a psql parancssori segédprogrammal PostgreSQL-kiszolgálóját.</span><span class="sxs-lookup"><span data-stu-id="36b75-171">The following example shows you how to successfully connect to your PostgreSQL server using the psql command-line utility.</span></span> <span data-ttu-id="36b75-172">Használja a `root.crt` fájl létrehozása és a `sslmode=verify-ca` vagy `sslmode=verify-full` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="36b75-172">Use the `root.crt` file created and the `sslmode=verify-ca` or `sslmode=verify-full` option.</span></span>

<span data-ttu-id="36b75-173">A PostgreSQL parancssori felületén, hajtsa végre a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="36b75-173">Using the PostgreSQL command-line interface, execute the following command:</span></span>
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mypgserver-20170401.postgres.database.azure.com dbname=postgres user=mylogin@mypgserver-20170401"
```
<span data-ttu-id="36b75-174">Ha sikeres, a következő eredmény jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="36b75-174">If successful, you receive the following output:</span></span>
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

#### <a name="using-pgadmin-gui-tool"></a><span data-ttu-id="36b75-175">Grafikus felhasználói Felülettel pgAdmin eszközzel</span><span class="sxs-lookup"><span data-stu-id="36b75-175">Using pgAdmin GUI tool</span></span>
<span data-ttu-id="36b75-176">Meg kell adnia pgAdmin biztonságos SSL Csatornán keresztül csatlakozni 4 konfigurálása számára a `SSL mode = Verify-CA` vagy `SSL mode = Verify-Full` az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="36b75-176">Configuring pgAdmin 4 to connect securely over SSL requires you to set the `SSL mode = Verify-CA` or `SSL mode = Verify-Full` as follows:</span></span>

![Képernyőkép a pgAdmin - kapcsolat – SSL-mód megkövetelése](./media/concepts-ssl-connection-security/2-pgadmin-ssl.png)

## <a name="next-steps"></a><span data-ttu-id="36b75-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="36b75-178">Next steps</span></span>
<span data-ttu-id="36b75-179">Tekintse át a következő különböző alkalmazás kapcsolati lehetőségek [adatkapcsolattárak PostgreSQL Azure-adatbázis](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="36b75-179">Review various application connectivity options following [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md)</span></span>
