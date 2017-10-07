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
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a><span data-ttu-id="09690-103">SSL-kapcsolatot PostgreSQL Azure-adatbázis konfigurálása</span><span class="sxs-lookup"><span data-stu-id="09690-103">Configure SSL connectivity in Azure Database for PostgreSQL</span></span>
<span data-ttu-id="09690-104">Azure-adatbázis PostgreSQL inkább csatlakozás a ügyfél alkalmazások toohello Secure Sockets Layer (SSL) PostgreSQL-szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="09690-104">Azure Database for PostgreSQL prefers connecting your client applications toohello PostgreSQL service using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="09690-105">Az adatbázis-kiszolgáló és az ügyfél alkalmazások közötti SSL-kapcsolatok kényszerítése megvédi a "hello középső man" támadások ellen hello adatfolyam hello kiszolgáló és az alkalmazás közötti titkosításával.</span><span class="sxs-lookup"><span data-stu-id="09690-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in hello middle" attacks by encrypting hello data stream between hello server and your application.</span></span>

<span data-ttu-id="09690-106">Alapértelmezés szerint a hello PostgreSQL-adatbázis szolgáltatás konfigurált toorequire SSL-kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="09690-106">By default, hello PostgreSQL database service is configured toorequire SSL connection.</span></span> <span data-ttu-id="09690-107">Másik lehetőségként letilthatja SSL megkövetelése tooyour adatbázis-szolgáltatás Kapcsolódás, ha az ügyfélalkalmazást nem támogatja az SSL-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="09690-107">Optionally, you can disable requiring SSL for connecting tooyour database service if your client application does not support SSL connectivity.</span></span> 

## <a name="enforcing-ssl-connections"></a><span data-ttu-id="09690-108">SSL-kapcsolatok kényszerítése</span><span class="sxs-lookup"><span data-stu-id="09690-108">Enforcing SSL connections</span></span>
<span data-ttu-id="09690-109">Az összes Azure Database hello Azure-portál és a parancssori felületen keresztül szerezhetők PostgreSQL-kiszolgálók esetén az SSL-kapcsolatok kényszerítési alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="09690-109">For all Azure Database for PostgreSQL servers provisioned through hello Azure portal and CLI, enforcement of SSL connections is enabled by default.</span></span> 

<span data-ttu-id="09690-110">Hasonlóképpen hello "Kapcsolati karakterláncok" beállításokat a kiszolgálón a hello Azure-portálon előre definiált kapcsolati karakterláncok tartalmaznak hello szükséges paraméterek közös nyelvek tooconnect tooyour adatbázis-kiszolgáló SSL-t használ.</span><span class="sxs-lookup"><span data-stu-id="09690-110">Likewise, connection strings that are pre-defined in hello "Connection Strings" settings under your server in hello Azure portal include hello required parameters for common languages tooconnect tooyour database server using SSL.</span></span> <span data-ttu-id="09690-111">hello SSL paraméter függ hello összekötő, például "ssl = true" vagy "sslmode = szükséges" vagy "sslmode = szükséges" és egyéb változatok.</span><span class="sxs-lookup"><span data-stu-id="09690-111">hello SSL parameter varies based on hello connector, for example "ssl=true" or "sslmode=require" or "sslmode=required" and other variations.</span></span>

## <a name="configure-enforcement-of-ssl"></a><span data-ttu-id="09690-112">Az SSL kényszerítésének</span><span class="sxs-lookup"><span data-stu-id="09690-112">Configure Enforcement of SSL</span></span>
<span data-ttu-id="09690-113">Igény szerint letilthatja végrehajtó SSL-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="09690-113">You can optionally disable enforcing SSL connectivity.</span></span> <span data-ttu-id="09690-114">A Microsoft Azure azt javasolja, hogy tooalways engedélyezése **kényszerítése SSL-kapcsolat** vonatkozó fokozott biztonsági beállításait.</span><span class="sxs-lookup"><span data-stu-id="09690-114">Microsoft Azure recommends tooalways enable **Enforce SSL connection** setting for enhanced security.</span></span>

### <a name="using-hello-azure-portal"></a><span data-ttu-id="09690-115">Hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="09690-115">Using hello Azure portal</span></span>
<span data-ttu-id="09690-116">Keresse fel az Azure-adatbázis PostgreSQL-kiszolgáló, és kattintson a **kapcsolatbiztonsági**.</span><span class="sxs-lookup"><span data-stu-id="09690-116">Visit your Azure Database for PostgreSQL server and click **Connection security**.</span></span> <span data-ttu-id="09690-117">Hello váltása gomb tooenable használja, vagy tiltsa le a hello **kényszerítése SSL-kapcsolat** beállítást.</span><span class="sxs-lookup"><span data-stu-id="09690-117">Use hello toggle button tooenable or disable hello **Enforce SSL connection** setting.</span></span> <span data-ttu-id="09690-118">Ezután kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="09690-118">Then click **Save**.</span></span> 

![Kapcsolat biztonsági - Disable SSL kényszerítése](./media/concepts-ssl-connection-security/1-disable-ssl.png)

<span data-ttu-id="09690-120">Hello megtekintésével ellenőrizheti a hello beállítás **áttekintése** lap toosee hello **SSL kényszerítése állapot** mutató.</span><span class="sxs-lookup"><span data-stu-id="09690-120">You can confirm hello setting by viewing hello **Overview** page toosee hello **SSL enforce status** indicator.</span></span>

### <a name="using-azure-cli"></a><span data-ttu-id="09690-121">Az Azure parancssori felület használata</span><span class="sxs-lookup"><span data-stu-id="09690-121">Using Azure CLI</span></span>
<span data-ttu-id="09690-122">Engedélyezheti vagy letilthatja a hello **ssl-kényszerítési** paraméter használatával `Enabled` vagy `Disabled` értékeket az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="09690-122">You can enable or disable hello **ssl-enforcement** parameter using `Enabled` or `Disabled` values respectively in Azure CLI.</span></span>

```azurecli
az postgres server update --resource-group myresourcegroup --name mypgserver-20170401 --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a><span data-ttu-id="09690-123">Ellenőrizze az alkalmazás vagy a keretrendszer támogatja SSL-kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="09690-123">Ensure your application or framework supports SSL connections</span></span>
<span data-ttu-id="09690-124">Sok általános alkalmazás-keretrendszert használó PostgreSQL az adatbázis-szolgáltatásokhoz, például a Drupal és a Django, ne engedélyezze az SSL alapértelmezés szerint telepítése során.</span><span class="sxs-lookup"><span data-stu-id="09690-124">Many common application frameworks that use PostgreSQL for database services, such as Drupal and Django, do not enable SSL by default during installation.</span></span> <span data-ttu-id="09690-125">Engedélyezi az SSL-kapcsolatot kell elvégezni, a telepítés után, vagy a parancssori felület parancsai adott toohello alkalmazáson keresztül.</span><span class="sxs-lookup"><span data-stu-id="09690-125">Enabling SSL connectivity must be done after installation or through CLI commands specific toohello application.</span></span> <span data-ttu-id="09690-126">Ha a PostgreSQL-kiszolgáló SSL-kapcsolatok kényszerít, és a kapcsolódó hello alkalmazás nincs megfelelően konfigurálva, a hello alkalmazás tooconnect tooyour adatbázis-kiszolgáló sikertelen lehet.</span><span class="sxs-lookup"><span data-stu-id="09690-126">If your PostgreSQL server is enforcing SSL connections and hello associated application is not configured properly, hello application may fail tooconnect tooyour database server.</span></span> <span data-ttu-id="09690-127">További részleteket az alkalmazás dokumentációjának toolearn hogyan tooenable SSL-kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="09690-127">Consult your application's documentation toolearn how tooenable SSL connections.</span></span>


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a><span data-ttu-id="09690-128">A tanúsítványellenőrzés az SSL-kapcsolatot igénylő alkalmazások</span><span class="sxs-lookup"><span data-stu-id="09690-128">Applications that require certificate verification for SSL connectivity</span></span>
<span data-ttu-id="09690-129">Bizonyos esetekben az alkalmazások által létrehozott egy megbízható tanúsítvány hitelesítésszolgáltatói (CA) tanúsítvány (.cer) fájl tooconnect biztonságosan helyi tanúsítványfájl igényelnek.</span><span class="sxs-lookup"><span data-stu-id="09690-129">In some cases, applications require a local certificate file generated from a trusted Certificate Authority (CA) certificate file (.cer) tooconnect securely.</span></span> <span data-ttu-id="09690-130">Tekintse meg a következő lépéseket tooobtain hello .cer fájl hello, hello tanúsítvány dekódolása, majd kösse tooyour alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="09690-130">See hello following steps tooobtain hello .cer file, decode hello certificate and bind it tooyour application.</span></span>

### <a name="download-hello-certificate-file-from-hello-certificate-authority-ca"></a><span data-ttu-id="09690-131">Hello tanúsítványfájl letöltését hello tanúsítvány hitelesítésszolgáltatói (CA)</span><span class="sxs-lookup"><span data-stu-id="09690-131">Download hello certificate file from hello Certificate Authority (CA)</span></span> 
<span data-ttu-id="09690-132">hello tanúsítvány szükséges toocommunicate az Azure-adatbázissal SSL-en keresztül PostgreSQL-kiszolgálót nem találnak [Itt](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt).</span><span class="sxs-lookup"><span data-stu-id="09690-132">hello certificate needed toocommunicate over SSL with your Azure Database for PostgreSQL server is located [here](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt).</span></span> <span data-ttu-id="09690-133">Töltse le a helyi hello tanúsítványfájlt.</span><span class="sxs-lookup"><span data-stu-id="09690-133">Download hello certificate file locally.</span></span>

### <a name="download-and-install-openssl-on-your-machine"></a><span data-ttu-id="09690-134">Töltse le és telepítse a OpenSSL a számítógépen</span><span class="sxs-lookup"><span data-stu-id="09690-134">Download and install OpenSSL on your machine</span></span> 
<span data-ttu-id="09690-135">az alkalmazás tooconnect biztonságosan szükséges toodecode hello tanúsítványfájl tooyour adatbázis-kiszolgáló, tooinstall OpenSSL a helyi számítógépre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="09690-135">toodecode hello certificate file needed for your application tooconnect securely tooyour database server, you need tooinstall OpenSSL on your local computer.</span></span>

#### <a name="for-linux-os-x-or-unix"></a><span data-ttu-id="09690-136">A Linux, OS X or Unix</span><span class="sxs-lookup"><span data-stu-id="09690-136">For Linux, OS X, or Unix</span></span>
<span data-ttu-id="09690-137">hello OpenSSL-függvénytárak szerepelnek közvetlenül hello forráskódjának [OpenSSL szoftver Foundation](http://www.openssl.org).</span><span class="sxs-lookup"><span data-stu-id="09690-137">hello OpenSSL libraries are provided in source code directly from hello [OpenSSL Software Foundation](http://www.openssl.org).</span></span> <span data-ttu-id="09690-138">hello következő utasítások végigvezetik hello lépéseket szükséges tooinstall OpenSSL a Linux rendszerű számítógépen.</span><span class="sxs-lookup"><span data-stu-id="09690-138">hello following instructions guide you through hello steps necessary tooinstall OpenSSL on your Linux PC.</span></span> <span data-ttu-id="09690-139">Ebben a cikkben az Ubuntu 12.04 és magasabb toowork ismert parancsok.</span><span class="sxs-lookup"><span data-stu-id="09690-139">This article uses commands known toowork on Ubuntu 12.04 and higher.</span></span>

<span data-ttu-id="09690-140">Nyisson meg egy terminál-munkamenetet, és OpenSSL telepítése</span><span class="sxs-lookup"><span data-stu-id="09690-140">Open a terminal session and install OpenSSL</span></span>
```bash
wget http://www.openssl.org/source/openssl-1.1.0e.tar.gz
``` 
<span data-ttu-id="09690-141">Csomagolja ki hello fájlokat hello letöltőcsomag</span><span class="sxs-lookup"><span data-stu-id="09690-141">Extract hello files from hello download package</span></span>
```bash
tar -xvzf openssl-1.1.0e.tar.gz
```
<span data-ttu-id="09690-142">Adja meg hello könyvtárat, amelyben hello fájlok könyvtárban találhatók.</span><span class="sxs-lookup"><span data-stu-id="09690-142">Enter hello directory where hello files were extracted.</span></span> <span data-ttu-id="09690-143">Alapértelmezés szerint az alábbinak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="09690-143">By default, it should be as follows.</span></span>

```bash
cd openssl-1.1.0e
```
<span data-ttu-id="09690-144">OpenSSL konfigurálása a következő hello a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="09690-144">Configure OpenSSL by executing hello following command.</span></span> <span data-ttu-id="09690-145">Ha egy mappa fájljait hello /usr/local/openssl eltér, győződjön meg arról, hogy toochange hello követő szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="09690-145">If you want hello files in a folder different than /usr/local/openssl, make sure toochange hello following as appropriate.</span></span>

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
<span data-ttu-id="09690-146">Most, hogy OpenSSL megfelelően van konfigurálva, toocompile kell azt tooconvert a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="09690-146">Now that OpenSSL is configured properly, you need toocompile it tooconvert your certificate.</span></span> <span data-ttu-id="09690-147">Futtassa a következő parancs hello toocompile:</span><span class="sxs-lookup"><span data-stu-id="09690-147">toocompile, run hello following command:</span></span>

```bash
make
```
<span data-ttu-id="09690-148">Ha fordítása befejeződött, most készen áll a tooinstall OpenSSL végrehajtható fájlként hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="09690-148">Once compiling is complete, you're ready tooinstall OpenSSL as an executable by running hello following command:</span></span>
```bash
make install
```
<span data-ttu-id="09690-149">tooconfirm, hogy sikeresen telepítette OpenSSL a rendszeren, futtatási hello következő parancsot, és arra, hogy hello toomake ellenőrizze azonos kimenethez.</span><span class="sxs-lookup"><span data-stu-id="09690-149">tooconfirm that you've successfully installed OpenSSL on your system, run hello following command and check toomake sure you get hello same output.</span></span>

```bash
/usr/local/openssl/bin/openssl version
```
<span data-ttu-id="09690-150">Ha sikeres hello a következő üzenetet kell látnia.</span><span class="sxs-lookup"><span data-stu-id="09690-150">If successful you should see hello following message.</span></span>
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

#### <a name="for-windows"></a><span data-ttu-id="09690-151">Windows rendszerhez</span><span class="sxs-lookup"><span data-stu-id="09690-151">For Windows</span></span>
<span data-ttu-id="09690-152">A következő módokon hello OpenSSL telepítése Windows rendszerű elvégezhető:</span><span class="sxs-lookup"><span data-stu-id="09690-152">Installing OpenSSL on a Windows PC can be done in hello following ways:</span></span>
1. <span data-ttu-id="09690-153">**(Ajánlott)**  Hello beépített Bash a Windows-szolgáltatást használ a Windows 10-es és újabb verziók, OpenSSL alapértelmezés szerint telepítve van-e.</span><span class="sxs-lookup"><span data-stu-id="09690-153">**(Recommended)** Using hello built-in Bash for Windows functionality in Window 10 and above, OpenSSL is installed by default.</span></span> <span data-ttu-id="09690-154">Hogyan tooenable Bash a Windows-funkciókat a Windows 10-es található utasításokat [Itt](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).</span><span class="sxs-lookup"><span data-stu-id="09690-154">Instructions on how tooenable Bash for Windows functionality in Windows 10 can be found [here](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).</span></span>
2. <span data-ttu-id="09690-155">A letöltés a Win32/64 alkalmazás hello közösségi által biztosított.</span><span class="sxs-lookup"><span data-stu-id="09690-155">Through downloading a Win32/64 application provided by hello community.</span></span> <span data-ttu-id="09690-156">Amíg nem adja meg a OpenSSL szoftver Foundation hello, vagy hagyja jóvá a megadott Windows Installer telepítők, rendelkezésre álló telepítők listáját tartalmazzák [Itt](https://wiki.openssl.org/index.php/Binaries)</span><span class="sxs-lookup"><span data-stu-id="09690-156">While hello OpenSSL Software Foundation does not provide or endorse any specific Windows installers, they provide a list of available installers [here](https://wiki.openssl.org/index.php/Binaries)</span></span>

### <a name="decode-your-certificate-file"></a><span data-ttu-id="09690-157">A tanúsítványfájl dekódolása</span><span class="sxs-lookup"><span data-stu-id="09690-157">Decode your certificate file</span></span>
<span data-ttu-id="09690-158">hello letöltött fájl nem titkosított formátumban legfelső szintű hitelesítésszolgáltató.</span><span class="sxs-lookup"><span data-stu-id="09690-158">hello downloaded Root CA file is in encrypted format.</span></span> <span data-ttu-id="09690-159">Használja a OpenSSL toodecode hello tanúsítványfájlt.</span><span class="sxs-lookup"><span data-stu-id="09690-159">Use OpenSSL toodecode hello certificate file.</span></span> <span data-ttu-id="09690-160">toodo Igen, a parancsot, OpenSSL:</span><span class="sxs-lookup"><span data-stu-id="09690-160">toodo so, run this OpenSSL command:</span></span>

```dos
OpenSSL>x509 -inform DER -in BaltimoreCyberTrustRoot.cer -text -out root.crt
```

### <a name="connecting-tooazure-database-for-postgresql-with-ssl-certificate-authentication"></a><span data-ttu-id="09690-161">Csatlakozás tooAzure adatbázis PostgreSQL SSL tanúsítványalapú hitelesítéssel ellátott</span><span class="sxs-lookup"><span data-stu-id="09690-161">Connecting tooAzure Database for PostgreSQL with SSL certificate authentication</span></span>
<span data-ttu-id="09690-162">Most, hogy sikeres rendelkeznek dekódolni, a tanúsítványt, csatlakoztathatja tooyour adatbázis-kiszolgáló biztonságos SSL-en keresztül.</span><span class="sxs-lookup"><span data-stu-id="09690-162">Now that you have successfully decoded your certificate, you can now connect tooyour database server securely over SSL.</span></span> <span data-ttu-id="09690-163">kiszolgáló-tanúsítvány ellenőrzés tooallow, hello fájl ~/.postgresql/root.crt a hello felhasználó kezdőkönyvtárának hello tanúsítványt kell helyezni.</span><span class="sxs-lookup"><span data-stu-id="09690-163">tooallow server certificate verification, hello certificate must be placed in hello file ~/.postgresql/root.crt in hello user's home directory.</span></span> <span data-ttu-id="09690-164">(A Microsoft Windows hello-fájl neve % APPDATA%\postgresql\root.crt.).</span><span class="sxs-lookup"><span data-stu-id="09690-164">(On Microsoft Windows hello file is named %APPDATA%\postgresql\root.crt.).</span></span> <span data-ttu-id="09690-165">hello következő tooAzure adatbázis csatlakozás PostgreSQL vonatkozó utasításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="09690-165">hello following provides instructions for connecting tooAzure Database for PostgreSQL.</span></span>

> [!NOTE]
> <span data-ttu-id="09690-166">Jelenleg egy ismert probléma használatakor "sslmode ellenőrizze teljes =" a kapcsolat toohello szolgáltatásban a hello kapcsolat nem jön létre a következő hiba hello: _kiszolgálótanúsítvány "&lt;régió&gt;. Control.Database.Windows.NET"(és más neveket 7) nem egyezik meg a gazdagép neve"&lt;kiszolgálónév&gt;. postgres.database.azure.com "._</span><span class="sxs-lookup"><span data-stu-id="09690-166">Currently, there is a known issue if you use "sslmode=verify-full" in your connection toohello service, hello connection will fail with hello following error: _server certificate for "&lt;region&gt;.control.database.windows.net" (and 7 other names) does not match host name "&lt;servername&gt;.postgres.database.azure.com"._</span></span>
> <span data-ttu-id="09690-167">Ha "sslmode ellenőrizze teljes =" van szükség esetén használjon hello server elnevezési  **&lt;kiszolgálónév&gt;. database.windows.net** a kapcsolati karakterlánc állomás neve.</span><span class="sxs-lookup"><span data-stu-id="09690-167">If "sslmode=verify-full" is required, please use hello server naming convention **&lt;servername&gt;.database.windows.net** as your connection string host name.</span></span> <span data-ttu-id="09690-168">Ez a korlátozás a jövőbeli hello tervezzük a tooremove.</span><span class="sxs-lookup"><span data-stu-id="09690-168">We plan tooremove this limitation in hello future.</span></span> <span data-ttu-id="09690-169">Egyéb használó kapcsolatok [SSL módok](https://www.postgresql.org/docs/9.6/static/libpq-ssl.html#LIBPQ-SSL-SSLMODE-STATEMENTS) toouse előnyben részesített hello állomás elnevezési konvenciót kell folytatódnia  **&lt;kiszolgálónév&gt;. postgres.database.azure.com**.</span><span class="sxs-lookup"><span data-stu-id="09690-169">Connections using other [SSL modes](https://www.postgresql.org/docs/9.6/static/libpq-ssl.html#LIBPQ-SSL-SSLMODE-STATEMENTS) should continue toouse hello preferred host naming convention **&lt;servername&gt;.postgres.database.azure.com**.</span></span>

#### <a name="using-psql-command-line-utility"></a><span data-ttu-id="09690-170">Psql parancssori segédprogrammal</span><span class="sxs-lookup"><span data-stu-id="09690-170">Using psql command-line utility</span></span>
<span data-ttu-id="09690-171">hello a következő példa bemutatja, hogyan kapcsolódnak az toosuccessfully a tooyour PostgreSQL server hello psql parancssori segédprogram segítségével.</span><span class="sxs-lookup"><span data-stu-id="09690-171">hello following example shows you how toosuccessfully connect tooyour PostgreSQL server using hello psql command-line utility.</span></span> <span data-ttu-id="09690-172">Használjon hello `root.crt` fájl létrehozása és hello `sslmode=verify-ca` vagy `sslmode=verify-full` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="09690-172">Use hello `root.crt` file created and hello `sslmode=verify-ca` or `sslmode=verify-full` option.</span></span>

<span data-ttu-id="09690-173">A következő parancs hello hello PostgreSQL parancssori felület segítségével hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="09690-173">Using hello PostgreSQL command-line interface, execute hello following command:</span></span>
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mypgserver-20170401.postgres.database.azure.com dbname=postgres user=mylogin@mypgserver-20170401"
```
<span data-ttu-id="09690-174">Ha sikeres, a következő kimeneti hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="09690-174">If successful, you receive hello following output:</span></span>
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

#### <a name="using-pgadmin-gui-tool"></a><span data-ttu-id="09690-175">Grafikus felhasználói Felülettel pgAdmin eszközzel</span><span class="sxs-lookup"><span data-stu-id="09690-175">Using pgAdmin GUI tool</span></span>
<span data-ttu-id="09690-176">PgAdmin 4 tooconnect biztonságosan konfigurálása SSL-en keresztül igényel tooset hello `SSL mode = Verify-CA` vagy `SSL mode = Verify-Full` az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="09690-176">Configuring pgAdmin 4 tooconnect securely over SSL requires you tooset hello `SSL mode = Verify-CA` or `SSL mode = Verify-Full` as follows:</span></span>

![Képernyőkép a pgAdmin - kapcsolat – SSL-mód megkövetelése](./media/concepts-ssl-connection-security/2-pgadmin-ssl.png)

## <a name="next-steps"></a><span data-ttu-id="09690-178">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="09690-178">Next steps</span></span>
<span data-ttu-id="09690-179">Tekintse át a következő különböző alkalmazás kapcsolati lehetőségek [adatkapcsolattárak PostgreSQL Azure-adatbázis](concepts-connection-libraries.md)</span><span class="sxs-lookup"><span data-stu-id="09690-179">Review various application connectivity options following [Connection libraries for Azure Database for PostgreSQL](concepts-connection-libraries.md)</span></span>
