---
title: "Az Azure Tártallózó hibaelhárítási útmutatója |} Microsoft Docs"
description: "A két funkció az Azure-hibakeresés áttekintése"
services: virtual-machines
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
ms.assetid: 
ms.service: virtual-machines
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: delhan
ms.openlocfilehash: e9b833b07556378f17d9aaff0912c7d73dff44eb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a><span data-ttu-id="83524-103">Az Azure Tártallózó hibaelhárítási útmutató</span><span class="sxs-lookup"><span data-stu-id="83524-103">Azure Storage Explorer troubleshooting guide</span></span>

## <a name="introduction"></a><span data-ttu-id="83524-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="83524-104">Introduction</span></span>

<span data-ttu-id="83524-105">A Microsoft Azure Tártallózó (előzetes verzió) egy különálló alkalmazás, amelynek segítségével egyszerűen dolgozhat Azure Storage-adatokkal Windows, a macOS és a Linux.</span><span class="sxs-lookup"><span data-stu-id="83524-105">Microsoft Azure Storage Explorer (Preview) is a stand-alone app that enables you to easily work with Azure Storage data on Windows, macOS and Linux.</span></span> <span data-ttu-id="83524-106">Az alkalmazás Azure állami felhők és Azure verem üzemeltetett toStorage fiókok is elérheti.</span><span class="sxs-lookup"><span data-stu-id="83524-106">The app can connect toStorage accounts hosted on Azure, Sovereign Clouds, and Azure Stack.</span></span>

<span data-ttu-id="83524-107">Ez az útmutató a megoldások a Tártallózó tapasztalt gyakori problémákat foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="83524-107">This guide summarizes solutions for common issues seen in Storage Explorer.</span></span>

## <a name="sign-in-issues"></a><span data-ttu-id="83524-108">Jelentkezzen be a problémák</span><span class="sxs-lookup"><span data-stu-id="83524-108">Sign in issues</span></span>

<span data-ttu-id="83524-109">Csak az Azure Active Directory (AAD) fiókok támogatottak.</span><span class="sxs-lookup"><span data-stu-id="83524-109">Only Azure Active Directory (AAD) accounts are supported.</span></span> <span data-ttu-id="83524-110">Ha az AD FS fiókot használjon, várható, hogy a Tártallózó bejelentkezés csatlakoztatás nem működik.</span><span class="sxs-lookup"><span data-stu-id="83524-110">If you use an ADFS account, it’s expected that signing in to Storage Explorer would not work.</span></span> <span data-ttu-id="83524-111">A folytatás előtt indítsa újra az alkalmazást, és hogy kell-e javítani a problémák.</span><span class="sxs-lookup"><span data-stu-id="83524-111">Before you continue, try restarting your application and see whether the problems can be fixed.</span></span>

### <a name="error-self-signed-certificate-in-certificate-chain"></a><span data-ttu-id="83524-112">Hiba: A tanúsítványláncában szereplő önaláírt tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="83524-112">Error: Self-Signed Certificate in Certificate Chain</span></span>

<span data-ttu-id="83524-113">Több oka miért merülhetnek fel ezt a hibát, és a leggyakrabban használt két lehetnek az okai a következők:</span><span class="sxs-lookup"><span data-stu-id="83524-113">There are several reasons why you may encounter this error, and the most common two reasons are as follows:</span></span>

1. <span data-ttu-id="83524-114">Az alkalmazás "transzparens proxyra", vagyis a kiszolgálóhoz (például a vállalati kiszolgálónak) elfogja a HTTPS-forgalmat, visszafejtése, és azt egy önaláírt tanúsítványt használ majd titkosítása keresztül csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="83524-114">The app is connected through a “transparent proxy”, which means a server (such as your company server) is intercepting HTTPS traffic, decrypting it, and then encrypting it using a self-signed certificate.</span></span>

2. <span data-ttu-id="83524-115">Egy alkalmazás, például víruskereső szoftver, amely egy önaláírt SSL-tanúsítvány van hogy megkapja a HTTPS-üzenetek futtatja.</span><span class="sxs-lookup"><span data-stu-id="83524-115">You are running an application, such as antivirus software, which is injecting a self-signed SSL certificate into the HTTPS messages that you receive.</span></span>

<span data-ttu-id="83524-116">Amikor Tártallózó egyik hibát észlel, azt már nem tudja, hogy a fogadott üzenet HTTPS van-e illetéktelenül.</span><span class="sxs-lookup"><span data-stu-id="83524-116">When Storage Explorer encounters one of the issues, it can no longer know whether the received HTTPS message is tampered.</span></span> <span data-ttu-id="83524-117">Ha az önaláírt tanúsítvány egy példányát, hogy a Tártallózó megbízható.</span><span class="sxs-lookup"><span data-stu-id="83524-117">If you have a copy of the self-signed certificate, you can let Storage Explorer trust it.</span></span> <span data-ttu-id="83524-118">Ha biztos a tanúsítvány ki van beszúrva, kövesse az alábbi lépéseket, és keresse meg:</span><span class="sxs-lookup"><span data-stu-id="83524-118">If you are unsure of who is injecting the certificate, follow these steps to find it:</span></span>

1. <span data-ttu-id="83524-119">Nyissa meg az SSL telepítése</span><span class="sxs-lookup"><span data-stu-id="83524-119">Install Open SSL</span></span>

    - <span data-ttu-id="83524-120">[Windows](https://slproweb.com/products/Win32OpenSSL.html) (könnyű verzióinak elegendőnek kell lennie)</span><span class="sxs-lookup"><span data-stu-id="83524-120">[Windows](https://slproweb.com/products/Win32OpenSSL.html) (any of the light versions should be sufficient)</span></span>

    - <span data-ttu-id="83524-121">Mac- és Linux: kell figyelembe venni az operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="83524-121">Mac and Linux: should be included with your operating system</span></span>

2. <span data-ttu-id="83524-122">Nyissa meg az SSL futtatása</span><span class="sxs-lookup"><span data-stu-id="83524-122">Run Open SSL</span></span>

    - <span data-ttu-id="83524-123">Windows: a telepítési könyvtár megnyitásához kattintson **/bin/**, majd kattintson duplán **openssl.exe**.</span><span class="sxs-lookup"><span data-stu-id="83524-123">Windows: open the installation directory, click **/bin/**, and then double-click **openssl.exe**.</span></span>
    - <span data-ttu-id="83524-124">Mac- és Linux: futtatása **openssl** terminálról.</span><span class="sxs-lookup"><span data-stu-id="83524-124">Mac and Linux: run **openssl** from a terminal.</span></span>

3. <span data-ttu-id="83524-125">Végrehajtás s_client - showcerts-csatlakozás microsoft.com:443</span><span class="sxs-lookup"><span data-stu-id="83524-125">Execute s_client -showcerts -connect microsoft.com:443</span></span>

4. <span data-ttu-id="83524-126">Keresse meg az önaláírt tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="83524-126">Look for self-signed certificates.</span></span> <span data-ttu-id="83524-127">Ha biztos benne, amelyek van látva saját aláírással, keressen tetszőleges helyre a tárgy ("%s") és a kiállító ("i:") azonos.</span><span class="sxs-lookup"><span data-stu-id="83524-127">If you are unsure which are self-signed, look for anywhere the subject ("s:") and issuer ("i:") are the same.</span></span>

5. <span data-ttu-id="83524-128">Miután megtalálta az összes önaláírt tanúsítványokat, mindegyikhez, másolja be minden-kra **---BEGIN CERTIFICATE---** való **---vége tanúsítvány---** egy új .cer kiterjesztésű fájlba.</span><span class="sxs-lookup"><span data-stu-id="83524-128">When you have found any self-signed certificates, for each one, copy and paste everything from and including **-----BEGIN CERTIFICATE-----** to **-----END CERTIFICATE-----** to a new .cer file.</span></span>

6. <span data-ttu-id="83524-129">Nyissa meg a Tártallózót, kattintson a **szerkesztése** > **SSL-tanúsítványok** > **importálási tanúsítványok**, és majd a fájlkiválasztóval található, válassza ki, majd nyissa meg a létrehozott .cer kiterjesztésű fájlokat.</span><span class="sxs-lookup"><span data-stu-id="83524-129">Open Storage Explorer, click **Edit** > **SSL Certificates** > **Import Certificates**, and then use the file picker to find, select, and open the .cer files that you created.</span></span>

<span data-ttu-id="83524-130">Ha nem talál meg minden önaláírt tanúsítványokat használ a fenti lépéseket, kapcsolatfelvétel a visszajelzés eszközzel további segítséget itt találhat.</span><span class="sxs-lookup"><span data-stu-id="83524-130">If you cannot find any self-signed certificates using the above steps, contact us through the feedback tool for more help.</span></span>

### <a name="unable-to-retrieve-subscriptions"></a><span data-ttu-id="83524-131">Nem sikerült beolvasni az előfizetések</span><span class="sxs-lookup"><span data-stu-id="83524-131">Unable to retrieve subscriptions</span></span>

<span data-ttu-id="83524-132">Ha nem tudja beolvasni az előfizetések sikeres bejelentkezés után, kövesse az alábbi lépéseket a probléma elhárítása érdekében:</span><span class="sxs-lookup"><span data-stu-id="83524-132">If you are unable to retrieve your subscriptions after you successfully sign in, follow these steps to troubleshoot this issue:</span></span>

- <span data-ttu-id="83524-133">Győződjön meg arról, hogy a fiók hozzáfér az előfizetések az Azure-portál bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="83524-133">Verify that your account has access to the subscriptions by signing into the Azure portal.</span></span>

- <span data-ttu-id="83524-134">Győződjön meg arról, hogy be van jelentkezve a megfelelő környezet (Azure, Azure Kína, Azure Németország, Azure Amerikai Egyesült államokbeli kormányzati vagy egyéni környezet vagy az Azure-verem) használatával.</span><span class="sxs-lookup"><span data-stu-id="83524-134">Make sure that you have signed in using the correct environment (Azure, Azure China, Azure Germany, Azure US Government, or Custom Environment/Azure Stack).</span></span>

- <span data-ttu-id="83524-135">Ha a rendszer proxy mögött, győződjön meg arról, hogy a Tártallózó proxy megfelelően van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="83524-135">If you are behind a proxy, make sure that you have configured the Storage Explorer proxy properly.</span></span>

- <span data-ttu-id="83524-136">Próbálja meg eltávolítani, és olvasása a következő fiók.</span><span class="sxs-lookup"><span data-stu-id="83524-136">Try removing and readding the account.</span></span>

- <span data-ttu-id="83524-137">Próbálja meg a következő fájlok eltávolítása a gyökérkönyvtár (Ez azt jelenti, hogy C:\Users\ContosoUser), majd újra adja hozzá a fiókhoz:</span><span class="sxs-lookup"><span data-stu-id="83524-137">Try deleting the following files from your root directory (that is, C:\Users\ContosoUser), and then re-adding the account:</span></span>

    - <span data-ttu-id="83524-138">.adalcache</span><span class="sxs-lookup"><span data-stu-id="83524-138">.adalcache</span></span>

    - <span data-ttu-id="83524-139">.devaccounts</span><span class="sxs-lookup"><span data-stu-id="83524-139">.devaccounts</span></span>

    - <span data-ttu-id="83524-140">.extaccounts</span><span class="sxs-lookup"><span data-stu-id="83524-140">.extaccounts</span></span>

- <span data-ttu-id="83524-141">Tekintse meg a fejlesztői eszközök konzol (az F12 billentyű megnyomásával) hibaüzeneteket a bejelentkezés során:</span><span class="sxs-lookup"><span data-stu-id="83524-141">Watch the developer tools console (by pressing F12) when you are signing in for any error messages:</span></span>

![Fejlesztői eszközök](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### <a name="unable-to-see-the-authentication-page"></a><span data-ttu-id="83524-143">Nem sikerült a hitelesítés oldal jelenik meg</span><span class="sxs-lookup"><span data-stu-id="83524-143">Unable to see the authentication page</span></span>

<span data-ttu-id="83524-144">Ha nem sikerül, a hitelesítés lap, kövesse az alábbi lépéseket a probléma elhárítása érdekében:</span><span class="sxs-lookup"><span data-stu-id="83524-144">If you are unable to see the authentication page, follow these steps to troubleshoot this issue:</span></span>

- <span data-ttu-id="83524-145">Attól függően, hogy a kapcsolat sebességétől a bejelentkezési lap betöltése, és a párbeszédpanel bezárása előtt legalább egy percig várjon egy ideig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="83524-145">Depending on the speed of your connection, it may take a while for the sign-in page to load, wait at least one minute before closing the authentication dialog box.</span></span>

- <span data-ttu-id="83524-146">Ha a rendszer proxy mögött, győződjön meg arról, hogy a Tártallózó proxy megfelelően van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="83524-146">If you are behind a proxy, make sure that you have configured the Storage Explorer proxy properly.</span></span>

- <span data-ttu-id="83524-147">A fejlesztői konzolján megtekintheti az F12 billentyű lenyomása mellett.</span><span class="sxs-lookup"><span data-stu-id="83524-147">View the developer console by pressing the F12 key.</span></span> <span data-ttu-id="83524-148">Tekintse meg a fejlesztői konzolján válaszát, és ellenőrizze, hogy található bármely clue ennek okát a hitelesítés nem működik.</span><span class="sxs-lookup"><span data-stu-id="83524-148">Watch the responses from the developer console and see whether you can find any clue for why authentication not working.</span></span>

### <a name="cannot-remove-account"></a><span data-ttu-id="83524-149">Fiók nem távolítható el.</span><span class="sxs-lookup"><span data-stu-id="83524-149">Cannot remove account</span></span>

<span data-ttu-id="83524-150">Ha nem tudja eltávolítani egy fiókot, vagy az újrahitelesítés elemre hivatkozás nem befolyásolja, kövesse az alábbi lépéseket a probléma elhárítása érdekében:</span><span class="sxs-lookup"><span data-stu-id="83524-150">If you are unable to remove an account, or if the reauthenticate link does not do anything, follow these steps to troubleshoot this issue:</span></span>

- <span data-ttu-id="83524-151">Próbálja meg a következő fájlok eltávolítása a gyökérkönyvtár, és ezután olvasása a következő fiók:</span><span class="sxs-lookup"><span data-stu-id="83524-151">Try deleting the following files from your root directory, and then readding the account:</span></span>

    - <span data-ttu-id="83524-152">.adalcache</span><span class="sxs-lookup"><span data-stu-id="83524-152">.adalcache</span></span>

    - <span data-ttu-id="83524-153">.devaccounts</span><span class="sxs-lookup"><span data-stu-id="83524-153">.devaccounts</span></span>

    - <span data-ttu-id="83524-154">.extaccounts</span><span class="sxs-lookup"><span data-stu-id="83524-154">.extaccounts</span></span>

- <span data-ttu-id="83524-155">Ha el szeretné távolítani a SAS csatlakoztatott tároló-erőforrások, törölje a következő fájlokat:</span><span class="sxs-lookup"><span data-stu-id="83524-155">If you want to remove SAS attached Storage resources, delete the following files:</span></span>

    - <span data-ttu-id="83524-156">A Windows %AppData%/StorageExplorer mappa</span><span class="sxs-lookup"><span data-stu-id="83524-156">%AppData%/StorageExplorer folder for Windows</span></span>

    - <span data-ttu-id="83524-157">/Felhasználók/ < sajat_nev >/Library/Applicaiton támogatási/StorageExplorer Mac rendszerre</span><span class="sxs-lookup"><span data-stu-id="83524-157">/Users/<your_name>/Library/Applicaiton SUpport/StorageExplorer for Mac</span></span>

    - <span data-ttu-id="83524-158">Linux ~/.config/StorageExplorer</span><span class="sxs-lookup"><span data-stu-id="83524-158">~/.config/StorageExplorer for Linux</span></span>

> [!NOTE]
>  <span data-ttu-id="83524-159">Adja meg újra a hitelesítő adatokat, ha törli ezeket a fájlokat kell.</span><span class="sxs-lookup"><span data-stu-id="83524-159">You will have to reenter all your credentials if you delete these files.</span></span>

## <a name="proxy-issues"></a><span data-ttu-id="83524-160">Proxy problémák</span><span class="sxs-lookup"><span data-stu-id="83524-160">Proxy issues</span></span>

<span data-ttu-id="83524-161">Először is győződjön meg arról, hogy minden helyesen-e a következő adatokat a megadott:</span><span class="sxs-lookup"><span data-stu-id="83524-161">First, make sure that the following information you entered are all correct:</span></span>

- <span data-ttu-id="83524-162">A proxykiszolgáló URL-cím és port száma</span><span class="sxs-lookup"><span data-stu-id="83524-162">The proxy URL and port number</span></span>

- <span data-ttu-id="83524-163">Felhasználónevet és jelszót, ha az szükséges a proxy</span><span class="sxs-lookup"><span data-stu-id="83524-163">Username and password if required by the proxy</span></span>

### <a name="common-solutions"></a><span data-ttu-id="83524-164">Közös megoldások</span><span class="sxs-lookup"><span data-stu-id="83524-164">Common solutions</span></span>

<span data-ttu-id="83524-165">Ha továbbra is problémákat tapasztal, kövesse az alábbi lépéseket a hibakereséshez:</span><span class="sxs-lookup"><span data-stu-id="83524-165">If you are still experiencing issues, follow these steps to troubleshoot them:</span></span>

- <span data-ttu-id="83524-166">Ha a proxy használata nélkül is csatlakozni az internethez, ellenőrizze, hogy működik-e a Tártallózó nélkül proxy-beállítások engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="83524-166">If you can connect to the Internet without using your proxy, verify that Storage Explorer works without proxy settings enabled.</span></span> <span data-ttu-id="83524-167">Ha ez a helyzet, előfordulhat, hogy a proxybeállítások problémát kell.</span><span class="sxs-lookup"><span data-stu-id="83524-167">If this is the case, there may be an issue with your proxy settings.</span></span> <span data-ttu-id="83524-168">A proxykiszolgáló rendszergazdájához, és a problémák azonosításához dolgozni.</span><span class="sxs-lookup"><span data-stu-id="83524-168">Work with your proxy administrator to identify the problems.</span></span>

- <span data-ttu-id="83524-169">Győződjön meg arról, hogy más alkalmazások, amelyek a proxykiszolgálót a várt módon működik-e.</span><span class="sxs-lookup"><span data-stu-id="83524-169">Verify that other applications using the proxy server work as expected.</span></span>

- <span data-ttu-id="83524-170">Ellenőrizze, hogy tud-e csatlakozni a webböngésző segítségével a Microsoft Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="83524-170">Verify that you can connect to the Microsoft Azure portal using your web browser</span></span>

- <span data-ttu-id="83524-171">Győződjön meg arról, hogy fogadhat válaszok a végpontok.</span><span class="sxs-lookup"><span data-stu-id="83524-171">Verify that you can receive responses from your service endpoints.</span></span> <span data-ttu-id="83524-172">Adja meg a végpont URL-címek egyikét a böngészőbe.</span><span class="sxs-lookup"><span data-stu-id="83524-172">Enter one of your endpoint URLs into your browser.</span></span> <span data-ttu-id="83524-173">Ha csatlakoztat, egy InvalidQueryParameterValue vagy hasonló XML-válasz kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="83524-173">If you can connect, you should receive an InvalidQueryParameterValue or similar XML response.</span></span>

- <span data-ttu-id="83524-174">Ha a proxykiszolgáló valaki más is használja a Tártallózó, győződjön meg arról, hogy csatlakozhassanak.</span><span class="sxs-lookup"><span data-stu-id="83524-174">If someone else is also using Storage Explorer with your proxy server, verify that they can connect.</span></span> <span data-ttu-id="83524-175">Ha a csatlakozás, előfordulhat, hogy kapcsolatba a proxy server rendszergazdával.</span><span class="sxs-lookup"><span data-stu-id="83524-175">If they can connect, you may have to contact your proxy server admin.</span></span>

### <a name="tools-for-diagnosing-issues"></a><span data-ttu-id="83524-176">A problémák diagnosztizálásával eszközök</span><span class="sxs-lookup"><span data-stu-id="83524-176">Tools for diagnosing issues</span></span>

<span data-ttu-id="83524-177">Ha hálózati eszközök, például a Fiddler a Windows, esetleg a problémák diagnosztizálásához az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="83524-177">If you have networking tools, such as Fiddler for Windows, you may be able to diagnose the problems as follows:</span></span>

- <span data-ttu-id="83524-178">Ha a proxyn keresztül történő működéséhez, előfordulhat, hogy a hálózati eszköz csatlakozni a proxyn keresztül történő konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="83524-178">If you have to work through your proxy, you may have to configure your networking tool to connect through the proxy.</span></span>

- <span data-ttu-id="83524-179">Ellenőrizze a hálózati eszköz által használt port számát.</span><span class="sxs-lookup"><span data-stu-id="83524-179">Check the port number used by your networking tool.</span></span>

- <span data-ttu-id="83524-180">Adja meg a helyi állomás URL-cím és a hálózati eszköz portszám Tártallózó proxybeállításai.</span><span class="sxs-lookup"><span data-stu-id="83524-180">Enter the local host URL and the networking tool's port number as proxy settings in Storage Explorer.</span></span> <span data-ttu-id="83524-181">Ebben az esetben megfelelően, ha a hálózati eszköz elindítja a hálózati kérelmek, felügyeleti és Szolgáltatásvégpontok Tártallózó által végzett naplózás.</span><span class="sxs-lookup"><span data-stu-id="83524-181">If this is done correctly, your networking tool starts logging network requests made by Storage Explorer to management and service endpoints.</span></span> <span data-ttu-id="83524-182">Írja be például a blob-végpontot egy böngészőben a https://cawablobgrs.blob.core.windows.net/, és le fogja kérni a válasz a következőhöz, ami alapján, az erőforrás létezik-e, bár nem férhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="83524-182">For example, enter https://cawablobgrs.blob.core.windows.net/ for your blob endpoint in a browser, and you will receive a response resembles the following, which suggests the resource exists, although you cannot access it.</span></span>

![kódminta](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a><span data-ttu-id="83524-184">Lépjen kapcsolatba a proxy-kiszolgálói rendszergazda</span><span class="sxs-lookup"><span data-stu-id="83524-184">Contact proxy server admin</span></span>

<span data-ttu-id="83524-185">Ha a proxybeállításai megfelelőek, előfordulhat, hogy a proxy server rendszergazdától, és</span><span class="sxs-lookup"><span data-stu-id="83524-185">If your proxy settings are correct, you may have to contact your proxy server admin, and</span></span>

- <span data-ttu-id="83524-186">Győződjön meg arról, hogy a proxy blokkolja Azure felügyeleti vagy erőforrás-végpontok irányuló forgalmat.</span><span class="sxs-lookup"><span data-stu-id="83524-186">Make sure that your proxy does not block traffic to Azure management or resource endpoints.</span></span>

- <span data-ttu-id="83524-187">Ellenőrizze a proxy-kiszolgáló által használt hitelesítési protokoll.</span><span class="sxs-lookup"><span data-stu-id="83524-187">Verify the authentication protocol used by your proxy server.</span></span> <span data-ttu-id="83524-188">A Tártallózó jelenleg nem támogatja az NTLM-proxyk.</span><span class="sxs-lookup"><span data-stu-id="83524-188">Storage Explorer does not currently support NTLM proxies.</span></span>

## <a name="unable-to-retrieve-children-error-message"></a><span data-ttu-id="83524-189">"Nem sikerült beolvasni a gyermekek" hibaüzenet jelenik meg</span><span class="sxs-lookup"><span data-stu-id="83524-189">"Unable to Retrieve Children" error message</span></span>

<span data-ttu-id="83524-190">Ha proxyn keresztül csatlakoznak az Azure-ba, győződjön meg arról, hogy a proxybeállítások helyességéről.</span><span class="sxs-lookup"><span data-stu-id="83524-190">If you are connected to Azure through a proxy, verify that your proxy settings are correct.</span></span> <span data-ttu-id="83524-191">Ha az előfizetés vagy a fiók tulajdonosának a hozzáférési volt engedélyezni lehessen egy erőforrást, győződjön meg arról, olvasási, vagy erőforrás engedélyeinek listázása</span><span class="sxs-lookup"><span data-stu-id="83524-191">If you were granted access to a resource from the owner of the subscription or account, verify that you have read or list permissions for that resource.</span></span>

### <a name="issues-with-sas-url"></a><span data-ttu-id="83524-192">SAS URL-cím problémái</span><span class="sxs-lookup"><span data-stu-id="83524-192">Issues with SAS URL</span></span>
<span data-ttu-id="83524-193">Ha a szolgáltatás egy SAS URL-cím segítségével, és ezt a hibát tapasztaló csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="83524-193">If you are connecting to a service using a SAS URL and experiencing this error:</span></span>

- <span data-ttu-id="83524-194">Győződjön meg arról, hogy az URL-cím biztosít-e olvasási és erőforrások sorolja fel a szükséges engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="83524-194">Verify that the URL provides the necessary permissions to read or list resources.</span></span>

- <span data-ttu-id="83524-195">Győződjön meg arról, hogy az URL-cím nem járt le.</span><span class="sxs-lookup"><span data-stu-id="83524-195">Verify that the URL has not expired.</span></span>

- <span data-ttu-id="83524-196">Ha a hozzáférési házirendek az SAS URL-cím alapú, győződjön meg arról, hogy a házirend nincs visszavonva.</span><span class="sxs-lookup"><span data-stu-id="83524-196">If the SAS URL is based on an access policy, verify that the access policy has not been revoked.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83524-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="83524-197">Next steps</span></span>

<span data-ttu-id="83524-198">Ha a megoldások egyike sem működik, az Ön, küldje el a problémát a visszajelzés eszközzel együtt az e-maileket, és annyi információkhoz juthat a problémáról, néven akkor is, így elküldhetjük Önnek a probléma.</span><span class="sxs-lookup"><span data-stu-id="83524-198">If none of the solutions work for you, submit your issue through the feedback tool with your email and as many details about the issue included as you can, so that we can contact you for fixing the issue.</span></span>

<span data-ttu-id="83524-199">Ehhez kattintson **súgó** menüben, majd kattintson **visszajelzés küldése**.</span><span class="sxs-lookup"><span data-stu-id="83524-199">To do this, click **Help** menu, and then click **Send Feedback**.</span></span>

![Visszajelzés](./media/storage-explorer-troubleshooting/4022503_en_1.png)
