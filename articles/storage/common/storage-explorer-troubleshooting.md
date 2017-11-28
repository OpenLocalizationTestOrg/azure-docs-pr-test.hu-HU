---
title: "aaaAzure Tártallózó hibaelhárítási útmutatója |} Microsoft Docs"
description: "Az Azure két hello hibakeresési szolgáltatása – áttekintés"
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
ms.date: 05/18/2017
ms.author: delhan
ms.openlocfilehash: 5152f70418707d65c0a4bce9a916336829956219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-explorer-troubleshooting-guide"></a><span data-ttu-id="85d07-103">Az Azure Tártallózó hibaelhárítási útmutató</span><span class="sxs-lookup"><span data-stu-id="85d07-103">Azure Storage Explorer troubleshooting guide</span></span>

## <a name="introduction"></a><span data-ttu-id="85d07-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="85d07-104">Introduction</span></span>

<span data-ttu-id="85d07-105">A Microsoft Azure Tártallózó (előzetes verzió) egy különálló alkalmazás, amely lehetővé teszi tooeasily dolgozhat Azure Storage-adatokkal Windows, a macOS és a Linux.</span><span class="sxs-lookup"><span data-stu-id="85d07-105">Microsoft Azure Storage Explorer (Preview) is a stand-alone app that enables you tooeasily work with Azure Storage data on Windows, macOS and Linux.</span></span> <span data-ttu-id="85d07-106">hello app Azure állami felhők és Azure verem üzemeltetett toStorage fiókok is elérheti.</span><span class="sxs-lookup"><span data-stu-id="85d07-106">hello app can connect toStorage accounts hosted on Azure, Sovereign Clouds, and Azure Stack.</span></span>

<span data-ttu-id="85d07-107">Ez az útmutató a megoldások a Tártallózó tapasztalt gyakori problémákat foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="85d07-107">This guide summarizes solutions for common issues seen in Storage Explorer.</span></span>

## <a name="sign-in-issues"></a><span data-ttu-id="85d07-108">Jelentkezzen be a problémák</span><span class="sxs-lookup"><span data-stu-id="85d07-108">Sign in issues</span></span>

<span data-ttu-id="85d07-109">A folytatás előtt indítsa újra az alkalmazást, és hogy kell-e javítani hello problémákat.</span><span class="sxs-lookup"><span data-stu-id="85d07-109">Before you continue, try restarting your application and see whether hello problems can be fixed.</span></span>

### <a name="error-self-signed-certificate-in-certificate-chain"></a><span data-ttu-id="85d07-110">Hiba: A tanúsítványláncában szereplő önaláírt tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="85d07-110">Error: Self-Signed Certificate in Certificate Chain</span></span>

<span data-ttu-id="85d07-111">Több oka miért merülhetnek fel ezt a hibát, és hello leggyakrabban használt két okok a következők:</span><span class="sxs-lookup"><span data-stu-id="85d07-111">There are several reasons why you may encounter this error, and hello most common two reasons are as follows:</span></span>

1. <span data-ttu-id="85d07-112">hello app "transzparens proxyra", vagyis a kiszolgálóhoz (például a vállalati kiszolgálónak) elfogja a HTTPS-forgalmat, visszafejtése, és azt egy önaláírt tanúsítványt használ majd titkosítása keresztül csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="85d07-112">hello app is connected through a “transparent proxy”, which means a server (such as your company server) is intercepting HTTPS traffic, decrypting it, and then encrypting it using a self-signed certificate.</span></span>

2. <span data-ttu-id="85d07-113">Egy alkalmazás, például víruskereső szoftver, amely egy önaláírt SSL-tanúsítvány van hogy fogadott hello HTTPS üzenetek futtatja.</span><span class="sxs-lookup"><span data-stu-id="85d07-113">You are running an application, such as antivirus software, which is injecting a self-signed SSL certificate into hello HTTPS messages that you receive.</span></span>

<span data-ttu-id="85d07-114">Amikor Tártallózó észlel hello problémák egyike, azt már nem tudja, hogy kapott HTTPS üdvözlőüzenetére van-e illetéktelenül.</span><span class="sxs-lookup"><span data-stu-id="85d07-114">When Storage Explorer encounters one of hello issues, it can no longer know whether hello received HTTPS message is tampered.</span></span> <span data-ttu-id="85d07-115">Ha hello önaláírt tanúsítvány egy példányát, hogy a Tártallózó megbízható.</span><span class="sxs-lookup"><span data-stu-id="85d07-115">If you have a copy of hello self-signed certificate, you can let Storage Explorer trust it.</span></span> <span data-ttu-id="85d07-116">Ha biztos ki van beszúrva hello tanúsítvány, kövesse ezeket a lépéseket toofind azt:</span><span class="sxs-lookup"><span data-stu-id="85d07-116">If you are unsure of who is injecting hello certificate, follow these steps toofind it:</span></span>

1. <span data-ttu-id="85d07-117">Nyissa meg az SSL telepítése</span><span class="sxs-lookup"><span data-stu-id="85d07-117">Install Open SSL</span></span>

    - <span data-ttu-id="85d07-118">[Windows](https://slproweb.com/products/Win32OpenSSL.html) (hello könnyű verzióinak elegendőnek kell lennie)</span><span class="sxs-lookup"><span data-stu-id="85d07-118">[Windows](https://slproweb.com/products/Win32OpenSSL.html) (any of hello light versions should be sufficient)</span></span>

    - <span data-ttu-id="85d07-119">Mac- és Linux: kell figyelembe venni az operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="85d07-119">Mac and Linux: should be included with your operating system</span></span>

2. <span data-ttu-id="85d07-120">Nyissa meg az SSL futtatása</span><span class="sxs-lookup"><span data-stu-id="85d07-120">Run Open SSL</span></span>

    - <span data-ttu-id="85d07-121">Windows: hello telepítési könyvtár megnyitásához kattintson **/bin/**, majd kattintson duplán **openssl.exe**.</span><span class="sxs-lookup"><span data-stu-id="85d07-121">Windows: open hello installation directory, click **/bin/**, and then double-click **openssl.exe**.</span></span>
    - <span data-ttu-id="85d07-122">Mac- és Linux: futtatása **openssl** terminálról.</span><span class="sxs-lookup"><span data-stu-id="85d07-122">Mac and Linux: run **openssl** from a terminal.</span></span>

3. <span data-ttu-id="85d07-123">Végrehajtás s_client - showcerts-csatlakozás microsoft.com:443</span><span class="sxs-lookup"><span data-stu-id="85d07-123">Execute s_client -showcerts -connect microsoft.com:443</span></span>

4. <span data-ttu-id="85d07-124">Keresse meg az önaláírt tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="85d07-124">Look for self-signed certificates.</span></span> <span data-ttu-id="85d07-125">Ha biztos benne, amelyeket önaláírt, keresse meg bárhol hello tulajdonos ("%s") és kibocsátó ("i:") van hello azonos.</span><span class="sxs-lookup"><span data-stu-id="85d07-125">If you are unsure which are self-signed, look for anywhere hello subject ("s:") and issuer ("i:") are hello same.</span></span>

5. <span data-ttu-id="85d07-126">Miután megtalálta az összes önaláírt tanúsítványokat, mindegyikhez, másolja be minden-kra **---BEGIN CERTIFICATE---** túl**---vége tanúsítvány---** tooa új .cer kiterjesztésű fájlba.</span><span class="sxs-lookup"><span data-stu-id="85d07-126">When you have found any self-signed certificates, for each one, copy and paste everything from and including **-----BEGIN CERTIFICATE-----** too**-----END CERTIFICATE-----** tooa new .cer file.</span></span>

6. <span data-ttu-id="85d07-127">Nyissa meg a Tártallózót, kattintson a **szerkesztése** > **SSL-tanúsítványok** > **importálási tanúsítványok**, és majd használata hello fájl objektumválasztó toofind, jelölje be, majd nyissa meg a létrehozott hello .cer kiterjesztésű fájlokat.</span><span class="sxs-lookup"><span data-stu-id="85d07-127">Open Storage Explorer, click **Edit** > **SSL Certificates** > **Import Certificates**, and then use hello file picker toofind, select, and open hello .cer files that you created.</span></span>

<span data-ttu-id="85d07-128">Ha bármely hello a fenti lépések használatával önaláírt tanúsítványok nem találja, kapcsolatfelvétel hello visszajelzés eszköz további segítséget itt találhat.</span><span class="sxs-lookup"><span data-stu-id="85d07-128">If you cannot find any self-signed certificates using hello above steps, contact us through hello feedback tool for more help.</span></span>

### <a name="unable-tooretrieve-subscriptions"></a><span data-ttu-id="85d07-129">Nem lehet tooretrieve előfizetések</span><span class="sxs-lookup"><span data-stu-id="85d07-129">Unable tooretrieve subscriptions</span></span>

<span data-ttu-id="85d07-130">Ha nem tooretrieve az előfizetések, miután sikeresen jelentkezzen be, kövesse ezeket a lépéseket tootroubleshoot probléma:</span><span class="sxs-lookup"><span data-stu-id="85d07-130">If you are unable tooretrieve your subscriptions after you successfully sign in, follow these steps tootroubleshoot this issue:</span></span>

- <span data-ttu-id="85d07-131">Győződjön meg arról, hogy a fiók rendelkezik-e hozzáférési toohello előfizetések Ha bejelentkezik hello Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="85d07-131">Verify that your account has access toohello subscriptions by signing into hello Azure Portal.</span></span>

- <span data-ttu-id="85d07-132">Győződjön meg arról, hogy bejelentkezett hello megfelelő környezet (Azure, Azure Kína, Azure Németország, Azure Amerikai Egyesült államokbeli kormányzati vagy egyéni környezet vagy az Azure-verem) használatával.</span><span class="sxs-lookup"><span data-stu-id="85d07-132">Make sure that you have signed in using hello correct environment (Azure, Azure China, Azure Germany, Azure US Government, or Custom Environment/Azure Stack).</span></span>

- <span data-ttu-id="85d07-133">Ha a rendszer proxy mögött, győződjön meg arról, hogy hello Tártallózó proxy megfelelően van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="85d07-133">If you are behind a proxy, make sure that you have configured hello Storage Explorer proxy properly.</span></span>

- <span data-ttu-id="85d07-134">Próbálja meg eltávolítani, és olvasása a következő hello fiók.</span><span class="sxs-lookup"><span data-stu-id="85d07-134">Try removing and readding hello account.</span></span>

- <span data-ttu-id="85d07-135">Törölje a következő fájlok a gyökérkönyvtárából (Ez azt jelenti, hogy C:\Users\ContosoUser), és majd ismételt felvételével hello fiók hello:</span><span class="sxs-lookup"><span data-stu-id="85d07-135">Try deleting hello following files from your root directory (that is, C:\Users\ContosoUser), and then re-adding hello account:</span></span>

    - <span data-ttu-id="85d07-136">.adalcache</span><span class="sxs-lookup"><span data-stu-id="85d07-136">.adalcache</span></span>

    - <span data-ttu-id="85d07-137">.devaccounts</span><span class="sxs-lookup"><span data-stu-id="85d07-137">.devaccounts</span></span>

    - <span data-ttu-id="85d07-138">.extaccounts</span><span class="sxs-lookup"><span data-stu-id="85d07-138">.extaccounts</span></span>

- <span data-ttu-id="85d07-139">A Watch hello fejlesztői eszközök konzol (F12 billentyű megnyomásával) amikor bejelentkezik a hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="85d07-139">Watch hello developer tools console (by pressing F12) when you are signing in for any error messages:</span></span>

![Fejlesztői eszközök](./media/storage-explorer-troubleshooting/4022501_en_2.png)

### <a name="unable-toosee-hello-authentication-page"></a><span data-ttu-id="85d07-141">Nem lehet toosee hello hitelesítés lap</span><span class="sxs-lookup"><span data-stu-id="85d07-141">Unable toosee hello authentication page</span></span>

<span data-ttu-id="85d07-142">Ha nem toosee hello hitelesítési lapot, kövesse ezeket a lépéseket tootroubleshoot probléma:</span><span class="sxs-lookup"><span data-stu-id="85d07-142">If you are unable toosee hello authentication page, follow these steps tootroubleshoot this issue:</span></span>

- <span data-ttu-id="85d07-143">Attól függően, hogy a kapcsolat sebességétől hello azt is igénybe vehet igénybe az hello bejelentkezési oldal tooload, hello párbeszédpanel bezárása előtt legalább egy percig várjon.</span><span class="sxs-lookup"><span data-stu-id="85d07-143">Depending on hello speed of your connection, it may take a while for hello sign-in page tooload, wait at least one minute before closing hello authentication dialog box.</span></span>

- <span data-ttu-id="85d07-144">Ha a rendszer proxy mögött, győződjön meg arról, hogy hello Tártallózó proxy megfelelően van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="85d07-144">If you are behind a proxy, make sure that you have configured hello Storage Explorer proxy properly.</span></span>

- <span data-ttu-id="85d07-145">Nézet hello fejlesztői konzolján hello F12 billentyű lenyomása mellett.</span><span class="sxs-lookup"><span data-stu-id="85d07-145">View hello developer console by pressing hello F12 key.</span></span> <span data-ttu-id="85d07-146">Hello válaszok hello fejlesztői konzolról nézze meg, és ellenőrizze, hogy található bármely clue ennek okát a hitelesítés nem működik.</span><span class="sxs-lookup"><span data-stu-id="85d07-146">Watch hello responses from hello developer console and see whether you can find any clue for why authentication not working.</span></span>

### <a name="cannot-remove-account"></a><span data-ttu-id="85d07-147">Fiók nem távolítható el.</span><span class="sxs-lookup"><span data-stu-id="85d07-147">Cannot remove account</span></span>

<span data-ttu-id="85d07-148">Ha Ön nem tooremove egy fiókot, vagy hello újrahitelesítés elemre hivatkozás nem befolyásolja, kövesse ezeket a lépéseket tootroubleshoot probléma:</span><span class="sxs-lookup"><span data-stu-id="85d07-148">If you are unable tooremove an account, or if hello reauthenticate link does not do anything, follow these steps tootroubleshoot this issue:</span></span>

- <span data-ttu-id="85d07-149">Törölje a következő fájlok a gyökérkönyvtárából, majd olvasása a következő hello fiók hello:</span><span class="sxs-lookup"><span data-stu-id="85d07-149">Try deleting hello following files from your root directory, and then readding hello account:</span></span>

    - <span data-ttu-id="85d07-150">.adalcache</span><span class="sxs-lookup"><span data-stu-id="85d07-150">.adalcache</span></span>

    - <span data-ttu-id="85d07-151">.devaccounts</span><span class="sxs-lookup"><span data-stu-id="85d07-151">.devaccounts</span></span>

    - <span data-ttu-id="85d07-152">.extaccounts</span><span class="sxs-lookup"><span data-stu-id="85d07-152">.extaccounts</span></span>

- <span data-ttu-id="85d07-153">Ha szeretné, hogy tooremove SAS csatlakoztatott tároló-erőforrások, törölje a következő fájlok hello:</span><span class="sxs-lookup"><span data-stu-id="85d07-153">If you want tooremove SAS attached Storage resources, delete hello following files:</span></span>

    - <span data-ttu-id="85d07-154">A Windows %AppData%/StorageExplorer mappa</span><span class="sxs-lookup"><span data-stu-id="85d07-154">%AppData%/StorageExplorer folder for Windows</span></span>

    - <span data-ttu-id="85d07-155">/Felhasználók/ < sajat_nev >/Library/Applicaiton támogatási/StorageExplorer Mac rendszerre</span><span class="sxs-lookup"><span data-stu-id="85d07-155">/Users/<your_name>/Library/Applicaiton SUpport/StorageExplorer for Mac</span></span>

    - <span data-ttu-id="85d07-156">Linux ~/.config/StorageExplorer</span><span class="sxs-lookup"><span data-stu-id="85d07-156">~/.config/StorageExplorer for Linux</span></span>

> [!NOTE]
>  <span data-ttu-id="85d07-157">Hogy tooreenter a hitelesítő adatok Ha törli ezeket a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="85d07-157">You will have tooreenter all your credentials if you delete these files.</span></span>

## <a name="proxy-issues"></a><span data-ttu-id="85d07-158">Proxy problémák</span><span class="sxs-lookup"><span data-stu-id="85d07-158">Proxy issues</span></span>

<span data-ttu-id="85d07-159">Először is győződjön meg arról, hogy hello a következő megadott adatok helyességét:</span><span class="sxs-lookup"><span data-stu-id="85d07-159">First, make sure that hello following information you entered are all correct:</span></span>

- <span data-ttu-id="85d07-160">hello proxy URL-cím és port száma</span><span class="sxs-lookup"><span data-stu-id="85d07-160">hello proxy URL and port number</span></span>

- <span data-ttu-id="85d07-161">Felhasználónév és jelszó igényei szerint hello proxy</span><span class="sxs-lookup"><span data-stu-id="85d07-161">Username and password if required by hello proxy</span></span>

### <a name="common-solutions"></a><span data-ttu-id="85d07-162">Közös megoldások</span><span class="sxs-lookup"><span data-stu-id="85d07-162">Common solutions</span></span>

<span data-ttu-id="85d07-163">Ha továbbra is problémákat tapasztal, kövesse ezeket a lépéseket tootroubleshoot őket:</span><span class="sxs-lookup"><span data-stu-id="85d07-163">If you are still experiencing issues, follow these steps tootroubleshoot them:</span></span>

- <span data-ttu-id="85d07-164">Toohello Internet a proxy használata nélkül is elérheti, győződjön meg arról, hogy működik-e a Tártallózó nélkül proxy-beállítások engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="85d07-164">If you can connect toohello Internet without using your proxy, verify that Storage Explorer works without proxy settings enabled.</span></span> <span data-ttu-id="85d07-165">Ha ez helyzet hello, valószínűleg a proxybeállítások kapcsolatos problémát.</span><span class="sxs-lookup"><span data-stu-id="85d07-165">If this is hello case, there may be an issue with your proxy settings.</span></span> <span data-ttu-id="85d07-166">A proxy felügyeleti tooidentify hello problémák működik.</span><span class="sxs-lookup"><span data-stu-id="85d07-166">Work with your proxy administrator tooidentify hello problems.</span></span>

- <span data-ttu-id="85d07-167">Győződjön meg arról, hogy más alkalmazások hello proxykiszolgáló használata a várt módon működik-e.</span><span class="sxs-lookup"><span data-stu-id="85d07-167">Verify that other applications using hello proxy server work as expected.</span></span>

- <span data-ttu-id="85d07-168">Gondoskodjon arról, hogy csatlakozhasson a webböngésző segítségével toohello Microsoft Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="85d07-168">Verify that you can connect toohello Microsoft Azure portal using your web browser</span></span>

- <span data-ttu-id="85d07-169">Győződjön meg arról, hogy fogadhat válaszok a végpontok.</span><span class="sxs-lookup"><span data-stu-id="85d07-169">Verify that you can receive responses from your service endpoints.</span></span> <span data-ttu-id="85d07-170">Adja meg a végpont URL-címek egyikét a böngészőbe.</span><span class="sxs-lookup"><span data-stu-id="85d07-170">Enter one of your endpoint URLs into your browser.</span></span> <span data-ttu-id="85d07-171">Ha csatlakoztat, egy InvalidQueryParameterValue vagy hasonló XML-válasz kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="85d07-171">If you can connect, you should receive an InvalidQueryParameterValue or similar XML response.</span></span>

- <span data-ttu-id="85d07-172">Ha a proxykiszolgáló valaki más is használja a Tártallózó, győződjön meg arról, hogy csatlakozhassanak.</span><span class="sxs-lookup"><span data-stu-id="85d07-172">If someone else is also using Storage Explorer with your proxy server, verify that they can connect.</span></span> <span data-ttu-id="85d07-173">Ha a csatlakozás, előfordulhat, hogy toocontact a proxy kiszolgáló rendszergazdájával.</span><span class="sxs-lookup"><span data-stu-id="85d07-173">If they can connect, you may have toocontact your proxy server admin.</span></span>

### <a name="tools-for-diagnosing-issues"></a><span data-ttu-id="85d07-174">A problémák diagnosztizálásával eszközök</span><span class="sxs-lookup"><span data-stu-id="85d07-174">Tools for diagnosing issues</span></span>

<span data-ttu-id="85d07-175">Ha hálózati eszközök, például a Fiddler a Windows, hogy az alábbiak szerint képes toodiagnose hello problémák merülhetnek fel:</span><span class="sxs-lookup"><span data-stu-id="85d07-175">If you have networking tools, such as Fiddler for Windows, you may be able toodiagnose hello problems as follows:</span></span>

- <span data-ttu-id="85d07-176">Ha a proxyn keresztül történő toowork, előfordulhat, hogy tooconfigure a hálózati eszköz tooconnect hello proxyn keresztül.</span><span class="sxs-lookup"><span data-stu-id="85d07-176">If you have toowork through your proxy, you may have tooconfigure your networking tool tooconnect through hello proxy.</span></span>

- <span data-ttu-id="85d07-177">Ellenőrizze a hálózati eszköz által használt hello portszámot.</span><span class="sxs-lookup"><span data-stu-id="85d07-177">Check hello port number used by your networking tool.</span></span>

- <span data-ttu-id="85d07-178">Adja meg a hello localhost URL-cím és hello hálózati eszköz portszámot a Tártallózó proxykiszolgáló-beállításként.</span><span class="sxs-lookup"><span data-stu-id="85d07-178">Enter hello local host URL and hello networking tool's port number as proxy settings in Storage Explorer.</span></span> <span data-ttu-id="85d07-179">Ha a isdone megfelelően, a hálózati eszköz elindul hálózati kérelmek Tártallózó toomanagement és a szolgáltatási végpont által végzett naplózás.</span><span class="sxs-lookup"><span data-stu-id="85d07-179">If this isdone correctly, your networking tool starts logging network requests made by Storage Explorer toomanagement and service endpoints.</span></span> <span data-ttu-id="85d07-180">Például adja meg a blob-végpontot egy böngészőben https://cawablobgrs.blob.core.windows.net/, és választ kap alábbihoz hello, ami alapján a hello erőforrás létezik-e, bár nem férhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="85d07-180">For example, enter https://cawablobgrs.blob.core.windows.net/ for your blob endpoint in a browser, and you will receive a response resembles hello following, which suggests hello resource exists, although you cannot access it.</span></span>

![kódminta](./media/storage-explorer-troubleshooting/4022502_en_2.png)

### <a name="contact-proxy-server-admin"></a><span data-ttu-id="85d07-182">Lépjen kapcsolatba a proxy-kiszolgálói rendszergazda</span><span class="sxs-lookup"><span data-stu-id="85d07-182">Contact proxy server admin</span></span>

<span data-ttu-id="85d07-183">Ha a proxybeállításai megfelelőek, előfordulhat, hogy toocontact a proxy server rendszer rendszergazdájához, és</span><span class="sxs-lookup"><span data-stu-id="85d07-183">If your proxy settings are correct, you may have toocontact your proxy server admin, and</span></span>

- <span data-ttu-id="85d07-184">Győződjön meg arról, hogy a proxy blokkolja forgalom tooAzure felügyeleti vagy erőforrás-végpontot.</span><span class="sxs-lookup"><span data-stu-id="85d07-184">Make sure that your proxy does not block traffic tooAzure management or resource endpoints.</span></span>

- <span data-ttu-id="85d07-185">Ellenőrizze a proxy server által használt hello hitelesítési protokoll.</span><span class="sxs-lookup"><span data-stu-id="85d07-185">Verify hello authentication protocol used by your proxy server.</span></span> <span data-ttu-id="85d07-186">A Tártallózó jelenleg nem támogatja az NTLM-proxyk.</span><span class="sxs-lookup"><span data-stu-id="85d07-186">Storage Explorer does not currently support NTLM proxies.</span></span>

## <a name="unable-tooretrieve-children-error-message"></a><span data-ttu-id="85d07-187">"Nem tooRetrieve gyermekek" hibaüzenet jelenik meg</span><span class="sxs-lookup"><span data-stu-id="85d07-187">"Unable tooRetrieve Children" error message</span></span>

<span data-ttu-id="85d07-188">Ha olyan proxyn keresztül csatlakoztatott tooAzure, győződjön meg arról, hogy a proxybeállítások helyességéről.</span><span class="sxs-lookup"><span data-stu-id="85d07-188">If you are connected tooAzure through a proxy, verify that your proxy settings are correct.</span></span> <span data-ttu-id="85d07-189">Hozzáférés tooa erőforrás kaptak hello tulajdonos hello előfizetés vagy a fiókot, ha győződjön meg arról, hogy elolvasta, vagy engedélyeket az adott erőforrás listában.</span><span class="sxs-lookup"><span data-stu-id="85d07-189">If you were granted access tooa resource from hello owner of hello subscription or account, verify that you have read or list permissions for that resource.</span></span>

### <a name="issues-with-sas-url"></a><span data-ttu-id="85d07-190">SAS URL-cím problémái</span><span class="sxs-lookup"><span data-stu-id="85d07-190">Issues with SAS URL</span></span>
<span data-ttu-id="85d07-191">Ha egy SAS URL-cím segítségével, és ezt a hibát tapasztaló tooa szolgáltatás csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="85d07-191">If you are connecting tooa service using a SAS URL and experiencing this error:</span></span>

- <span data-ttu-id="85d07-192">Győződjön meg arról, hogy hello URL-cím biztosítanak hello szükséges engedélyek tooread vagy a lista erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="85d07-192">Verify that hello URL provides hello necessary permissions tooread or list resources.</span></span>

- <span data-ttu-id="85d07-193">Győződjön meg arról, hogy hello URL-cím nem járt le.</span><span class="sxs-lookup"><span data-stu-id="85d07-193">Verify that hello URL has not expired.</span></span>

- <span data-ttu-id="85d07-194">Hello SAS URL-cím alapú hozzáférési házirend, győződjön meg arról, hogy hello házirend nincs visszavonva.</span><span class="sxs-lookup"><span data-stu-id="85d07-194">If hello SAS URL is based on an access policy, verify that hello access policy has not been revoked.</span></span>

## <a name="next-steps"></a><span data-ttu-id="85d07-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="85d07-195">Next steps</span></span>

<span data-ttu-id="85d07-196">Ha hello megoldások egyike sem működik, az Ön, küldje el a problémát az e-mail hello visszajelzés eszközzel, és annyi részleteit tartalmazza, ha Ön hello probléma is, így elküldhetjük Önnek a hello a probléma kijavítása.</span><span class="sxs-lookup"><span data-stu-id="85d07-196">If none of hello solutions work for you, submit your issue through hello feedback tool with your email and as many details about hello issue included as you can, so that we can contact you for fixing hello issue.</span></span>

<span data-ttu-id="85d07-197">toodo, kattintson **súgó** menüben, majd kattintson **visszajelzés küldése**.</span><span class="sxs-lookup"><span data-stu-id="85d07-197">toodo this, click **Help** menu, and then click **Send Feedback**.</span></span>

![Visszajelzés](./media/storage-explorer-troubleshooting/4022503_en_1.png)
