---
title: "aaaTroubleshoot az adatkezelési átjáró problémák |} Microsoft Docs"
description: "Tippek tootroubleshoot problémák kapcsolódó tooData adatkezelési átjáró biztosít."
services: data-factory
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: c6756c37-4e5a-4d1e-ab52-365f149b4128
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: abnarain
published: True
ms.openlocfilehash: 85dacc8a1e8d574d6e7d5b556c995cebdc148fde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-issues-with-using-data-management-gateway"></a><span data-ttu-id="f6227-103">Az adatkezelési átjáró használata közben felmerülő hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="f6227-103">Troubleshoot issues with using Data Management Gateway</span></span>
<span data-ttu-id="f6227-104">Ez a cikk tájékoztatást nyújt az adatkezelési átjáró használatával kapcsolatos hibák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="f6227-104">This article provides information on troubleshooting issues with using Data Management Gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="f6227-105">Lásd: hello [az adatkezelési átjáró](data-factory-data-management-gateway.md) cikk hello átjáró kapcsolatos részletes információkat.</span><span class="sxs-lookup"><span data-stu-id="f6227-105">See hello [Data Management Gateway](data-factory-data-management-gateway.md) article for detailed information about hello gateway.</span></span> <span data-ttu-id="f6227-106">Lásd: hello [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) adatok áthelyezése egy helyi SQL Server adatbázis tooMicrosoft Azure Blob Storage tárolóban hello átjáró használatával kapcsolatos általános bemutatóért cikkében.</span><span class="sxs-lookup"><span data-stu-id="f6227-106">See hello [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for a walkthrough of moving data from an on-premises SQL Server database tooMicrosoft Azure Blob storage by using hello gateway.</span></span>
>
>

## <a name="failed-tooinstall-or-register-gateway"></a><span data-ttu-id="f6227-107">Nem sikerült tooinstall vagy regisztrálni átjáró</span><span class="sxs-lookup"><span data-stu-id="f6227-107">Failed tooinstall or register gateway</span></span>
### <a name="1-problem"></a><span data-ttu-id="f6227-108">1. Probléma</span><span class="sxs-lookup"><span data-stu-id="f6227-108">1. Problem</span></span>
<span data-ttu-id="f6227-109">Megjelenik a hibaüzenet, amikor telepítése és regisztrálása az átjáró pontosabban hello átjáró telepítési fájl letöltése során.</span><span class="sxs-lookup"><span data-stu-id="f6227-109">You see this error message when installing and registering a gateway, specifically, while downloading hello gateway installation file.</span></span>

`Unable tooconnect toohello remote server". Please check your local settings (Error Code: 10003).`

#### <a name="cause"></a><span data-ttu-id="f6227-110">Ok</span><span class="sxs-lookup"><span data-stu-id="f6227-110">Cause</span></span>
<span data-ttu-id="f6227-111">hello gép tooinstall hello átjáró próbált toodownload hello legújabb átjáró telepítési fájlját a letöltőközpontból hello tooa hálózati probléma miatt nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="f6227-111">hello machine on which you are trying tooinstall hello gateway has failed toodownload hello latest gateway installation file from hello download center due tooa network issue.</span></span>

#### <a name="resolution"></a><span data-ttu-id="f6227-112">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="f6227-112">Resolution</span></span>
<span data-ttu-id="f6227-113">A tűzfal-proxy kiszolgáló beállítások toosee ellenőrizheti, hogy hello-beállítások le hello hálózati kapcsolat a hello számítógép toohello [letöltőközpontból](https://download.microsoft.com/), hello-beállítások frissítése, és ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="f6227-113">Check your firewall proxy server settings toosee whether hello settings block hello network connection from hello computer toohello [download center](https://download.microsoft.com/), and update hello settings accordingly.</span></span>

<span data-ttu-id="f6227-114">Alternatív megoldásként letöltheti hello telepítőfájlja legújabb átjáró hello hello [letöltőközpontból](https://www.microsoft.com/download/details.aspx?id=39717) hello letöltőközpontból hozzáférő más számítógépeken.</span><span class="sxs-lookup"><span data-stu-id="f6227-114">Alternatively, you can download hello installation file for hello latest gateway from hello [download center](https://www.microsoft.com/download/details.aspx?id=39717) on other machines that can access hello download center.</span></span> <span data-ttu-id="f6227-115">Ezután a másolási hello installer fájl toohello átjáró állomás, és futtassa manuálisan tooinstall és frissítés hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="f6227-115">You can then copy hello installer file toohello gateway host computer and run it manually tooinstall and update hello gateway.</span></span>

### <a name="2-problem"></a><span data-ttu-id="f6227-116">2. Probléma</span><span class="sxs-lookup"><span data-stu-id="f6227-116">2. Problem</span></span>
<span data-ttu-id="f6227-117">Ezt a hibaüzenetet látja, amikor egy átjáró tooinstall kattintva végrehajtani kívánt **telepíthető közvetlenül a számítógépen** a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f6227-117">You see this error when you're attempting tooinstall a gateway by clicking **install directly on this computer** in hello Azure portal.</span></span>

`Error:  Abort installing a new gateway on this computer because this computer has an existing installed gateway and a computer without any installed gateway is required for installing a new gateway.`  

#### <a name="cause"></a><span data-ttu-id="f6227-118">Ok</span><span class="sxs-lookup"><span data-stu-id="f6227-118">Cause</span></span>
<span data-ttu-id="f6227-119">Átjáró hello számítógépen már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="f6227-119">A gateway is already installed on hello machine.</span></span>

#### <a name="resolution"></a><span data-ttu-id="f6227-120">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="f6227-120">Resolution</span></span>
<span data-ttu-id="f6227-121">Távolítsa el a meglévő átjáró hello hello gépen, és kattintson a hello **telepíthető közvetlenül a számítógépen** újra hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="f6227-121">Uninstall hello existing gateway on hello machine and click hello **install directly on this computer** link again.</span></span>

### <a name="3-problem"></a><span data-ttu-id="f6227-122">3. Probléma</span><span class="sxs-lookup"><span data-stu-id="f6227-122">3. Problem</span></span>
<span data-ttu-id="f6227-123">Ez a hiba jelenhet meg, amikor egy új átjáró regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="f6227-123">You might see this error when registering a new gateway.</span></span>

`Error: hello gateway has encountered an error during registration.`

#### <a name="cause"></a><span data-ttu-id="f6227-124">Ok</span><span class="sxs-lookup"><span data-stu-id="f6227-124">Cause</span></span>
<span data-ttu-id="f6227-125">Láthatja, hogy ez az üzenet hello a következő okok valamelyike miatt:</span><span class="sxs-lookup"><span data-stu-id="f6227-125">You might see this message for one of hello following reasons:</span></span>

* <span data-ttu-id="f6227-126">hello átjárókulcs hello formátuma érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="f6227-126">hello format of hello gateway key is invalid.</span></span>
* <span data-ttu-id="f6227-127">hello átjárókulcs érvénytelenné vált.</span><span class="sxs-lookup"><span data-stu-id="f6227-127">hello gateway key has been invalidated.</span></span>
* <span data-ttu-id="f6227-128">hello átjárókulcs újra lett létrehozza a rendszer hello portálról.</span><span class="sxs-lookup"><span data-stu-id="f6227-128">hello gateway key has been regenerated from hello portal.</span></span>  

#### <a name="resolution"></a><span data-ttu-id="f6227-129">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="f6227-129">Resolution</span></span>
<span data-ttu-id="f6227-130">Győződjön meg arról, hogy használ-e hello jobb átjárókulcs hello portálról.</span><span class="sxs-lookup"><span data-stu-id="f6227-130">Verify whether you are using hello right gateway key from hello portal.</span></span> <span data-ttu-id="f6227-131">Ha szükséges, a kulcs újragenerálása és hello kulcs tooregister hello átjáró használatára.</span><span class="sxs-lookup"><span data-stu-id="f6227-131">If needed, regenerate a key and use hello key tooregister hello gateway.</span></span>

### <a name="4-problem"></a><span data-ttu-id="f6227-132">4. Probléma</span><span class="sxs-lookup"><span data-stu-id="f6227-132">4. Problem</span></span>
<span data-ttu-id="f6227-133">Hello átjáró regisztrálásakor még a következő hibaüzenet jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="f6227-133">You might see hello following error message when you're registering a gateway.</span></span>

`Error: hello content or format of hello gateway key "{gatewayKey}" is invalid, please go tooazure portal toocreate one new gateway or regenerate hello gateway key.`



![Tartalom és a kulcs formátuma érvénytelen](media/data-factory-troubleshoot-gateway-issues/invalid-format-gateway-key.png)

#### <a name="cause"></a><span data-ttu-id="f6227-135">Ok</span><span class="sxs-lookup"><span data-stu-id="f6227-135">Cause</span></span>
<span data-ttu-id="f6227-136">hello tartalom vagy hello bemeneti átjárókulcs formátuma nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="f6227-136">hello content or format of hello input gateway key is incorrect.</span></span> <span data-ttu-id="f6227-137">Hello okok valamelyike miatt lehet, hogy a másolt hello kulcs egy részének hello portálról, vagy érvénytelen kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="f6227-137">One of hello reasons can be that you copied only a portion of hello key from hello portal or you're using an invalid key.</span></span>

#### <a name="resolution"></a><span data-ttu-id="f6227-138">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="f6227-138">Resolution</span></span>
<span data-ttu-id="f6227-139">Hozzon létre egy átjáró kulcsot hello portálon, és hello Másolás gombra toocopy hello teljes kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="f6227-139">Generate a gateway key in hello portal, and use hello copy button toocopy hello whole key.</span></span> <span data-ttu-id="f6227-140">Illessze be az ablakot tooregister hello átjáró majd.</span><span class="sxs-lookup"><span data-stu-id="f6227-140">Then paste it in this window tooregister hello gateway.</span></span>

### <a name="5-problem"></a><span data-ttu-id="f6227-141">5. Probléma</span><span class="sxs-lookup"><span data-stu-id="f6227-141">5. Problem</span></span>
<span data-ttu-id="f6227-142">Hello átjáró regisztrálásakor még a következő hibaüzenet jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="f6227-142">You might see hello following error message when you're registering a gateway.</span></span>

`Error: hello gateway key is invalid or empty. Specify a valid gateway key from hello portal.`

![Átjáró kulcsa érvénytelen vagy üres:](media/data-factory-troubleshoot-gateway-issues/gateway-key-is-invalid-or-empty.png)

#### <a name="cause"></a><span data-ttu-id="f6227-144">Ok</span><span class="sxs-lookup"><span data-stu-id="f6227-144">Cause</span></span>
<span data-ttu-id="f6227-145">hello átjárókulcs újra lett létrehozva. vagy hello átjáró törölve lett az hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f6227-145">hello gateway key has been regenerated or hello gateway has been deleted in hello Azure portal.</span></span> <span data-ttu-id="f6227-146">Akkor is előfordulhat, ha nincs legújabb hello az adatkezelési átjáró beállítása.</span><span class="sxs-lookup"><span data-stu-id="f6227-146">It can also happen if hello Data Management Gateway setup is not latest.</span></span>

#### <a name="resolution"></a><span data-ttu-id="f6227-147">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="f6227-147">Resolution</span></span>
<span data-ttu-id="f6227-148">Ellenőrizze, hogy ha hello az adatkezelési átjáró telepítő hello legújabb verzióját, a hello Microsoft hello legújabb verziója található [letöltőközpontból](https://go.microsoft.com/fwlink/p/?LinkId=271260).</span><span class="sxs-lookup"><span data-stu-id="f6227-148">Check if hello Data Management Gateway setup is hello latest version, you can find hello latest version on hello Microsoft [download center](https://go.microsoft.com/fwlink/p/?LinkId=271260).</span></span>

<span data-ttu-id="f6227-149">Beállítás aktuális / legújabb és átjáró még létezik a portálon, ha az Azure-portálon hello hello átjáró kulcs újragenerálása és hello Másolás gombra toocopy hello teljes kulcsot használ, és majd illessze be az ablakot tooregister hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="f6227-149">If setup is current/ latest and gateway still exists on Portal, regenerate hello gateway key in hello Azure portal, and use hello copy button toocopy hello whole key, and then paste it in this window tooregister hello gateway.</span></span> <span data-ttu-id="f6227-150">Ellenkező esetben hozza létre újra a hello átjáró, és kezdje újra a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="f6227-150">Otherwise, recreate hello gateway and start over.</span></span>

### <a name="6-problem"></a><span data-ttu-id="f6227-151">6. Probléma</span><span class="sxs-lookup"><span data-stu-id="f6227-151">6. Problem</span></span>
<span data-ttu-id="f6227-152">Hello átjáró regisztrálásakor még a következő hibaüzenet jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="f6227-152">You might see hello following error message when you're registering a gateway.</span></span>

`Error: Gateway has been online for a while, then shows “Gateway is not registered” with hello status “Gateway key is invalid”`

![Átjáró kulcsa érvénytelen vagy üres:](media/data-factory-troubleshoot-gateway-issues/gateway-not-registered-key-invalid.png)

#### <a name="cause"></a><span data-ttu-id="f6227-154">Ok</span><span class="sxs-lookup"><span data-stu-id="f6227-154">Cause</span></span>
<span data-ttu-id="f6227-155">Ez a hiba akkor fordulhat elő, mert hello átjáró törölve lett vagy hello társított átjárókulcs újra lett létrehozva..</span><span class="sxs-lookup"><span data-stu-id="f6227-155">This error might happen because either hello gateway has been deleted or hello associated gateway key has been regenerated.</span></span>

#### <a name="resolution"></a><span data-ttu-id="f6227-156">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="f6227-156">Resolution</span></span>
<span data-ttu-id="f6227-157">Ha hello átjáró törölve lett, hozza létre újból hello átjáró hello portálról, kattintson a **regisztrálása**, hello kulcs másolása hello portálról, illessze be és próbálja meg tooregister hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="f6227-157">If hello gateway has been deleted, re-create hello gateway from hello portal, click **Register**, copy hello key from hello portal, paste it, and try tooregister hello gateway.</span></span>

<span data-ttu-id="f6227-158">Ha hello átjáró még létezik, de a kulcsot újra lett létrehozva., hello új kulcs tooregister hello átjáró használatára.</span><span class="sxs-lookup"><span data-stu-id="f6227-158">If hello gateway still exists but its key has been regenerated, use hello new key tooregister hello gateway.</span></span> <span data-ttu-id="f6227-159">Ha nem rendelkezik hello kulccsal, generálja újra hello kulcsot újra hello portálról.</span><span class="sxs-lookup"><span data-stu-id="f6227-159">If you don’t have hello key, regenerate hello key again from hello portal.</span></span>

### <a name="7-problem"></a><span data-ttu-id="f6227-160">7. Probléma</span><span class="sxs-lookup"><span data-stu-id="f6227-160">7. Problem</span></span>
<span data-ttu-id="f6227-161">Amikor egy átjáró regisztrálás szükség lehet tooenter elérési útját és a jelszó tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="f6227-161">When you're registering a gateway, you might need tooenter path and password for a certificate.</span></span>

![Adja meg a tanúsítvány](media/data-factory-troubleshoot-gateway-issues/specify-certificate.png)

#### <a name="cause"></a><span data-ttu-id="f6227-163">Ok</span><span class="sxs-lookup"><span data-stu-id="f6227-163">Cause</span></span>
<span data-ttu-id="f6227-164">hello átjáró előtt más gépeken regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="f6227-164">hello gateway has been registered on other machines before.</span></span> <span data-ttu-id="f6227-165">Hello kezdeti regisztrációs átjáró, során hello átjáró társítva lett egy titkosítási tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="f6227-165">During hello initial registration of a gateway, an encryption certificate has been associated with hello gateway.</span></span> <span data-ttu-id="f6227-166">hello tanúsítványt kell önálló hello átjáró által létrehozott vagy hello felhasználó által megadott.</span><span class="sxs-lookup"><span data-stu-id="f6227-166">hello certificate can either be self-generated by hello gateway or provided by hello user.</span></span>  <span data-ttu-id="f6227-167">Ez a tanúsítvány használt tooencrypt hello adattár (társított szolgáltatás) hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="f6227-167">This certificate is used tooencrypt credentials of hello data store (linked service).</span></span>  

![Tanúsítvány exportálása](media/data-factory-troubleshoot-gateway-issues/export-certificate.png)

<span data-ttu-id="f6227-169">Ha hello átjárót egy másik gazdaszámítógépet, hello regisztrációs varázsló megkérdezi, hogy a tanúsítvány toodecrypt korábban hitelesítő adatok a titkosított visszaállítása a tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="f6227-169">When restoring hello gateway on a different host machine, hello registration wizard asks for this certificate toodecrypt credentials previously encrypted with this certificate.</span></span>  <span data-ttu-id="f6227-170">Ez a tanúsítvány nélkül hello hitelesítő adatok nem tudja visszafejteni hello új átjáró, és az új átjáró társított későbbi másolási tevékenység végrehajtások sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="f6227-170">Without this certificate, hello credentials cannot be decrypted by hello new gateway and subsequent copy activity executions associated with this new gateway will fail.</span></span>  

#### <a name="resolution"></a><span data-ttu-id="f6227-171">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="f6227-171">Resolution</span></span>
<span data-ttu-id="f6227-172">Ha exportálása hello hitelesítő adatainak tanúsítványa hello eredeti átjáró gépen hello segítségével **exportálása** hello gombjára **beállítások** az adatkezelési átjáró konfigurációkezelőjének lapján hello használata Itt szerepel tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="f6227-172">If you have exported hello credential certificate from hello original gateway machine by using hello **Export** button on hello **Settings** tab in Data Management Gateway Configuration Manager, use hello certificate here.</span></span>

<span data-ttu-id="f6227-173">Ebben a szakaszban nem hagyható ki, amikor egy átjáró helyreállítása.</span><span class="sxs-lookup"><span data-stu-id="f6227-173">You cannot skip this stage when recovering a gateway.</span></span> <span data-ttu-id="f6227-174">Ha hello tanúsítványa hiányzik, toodelete hello átjáró hello portálról kell, és hozza létre az új átjárón.</span><span class="sxs-lookup"><span data-stu-id="f6227-174">If hello certificate is missing, you need toodelete hello gateway from hello portal and re-create a new gateway.</span></span>  <span data-ttu-id="f6227-175">Emellett frissítse az összes társított szolgáltatások, amelyek kapcsolódó toohello átjáró által ismét be kell írni a hitelesítő adataikat.</span><span class="sxs-lookup"><span data-stu-id="f6227-175">In addition, update all linked services that are related toohello gateway by reentering their credentials.</span></span>

### <a name="8-problem"></a><span data-ttu-id="f6227-176">8. Probléma</span><span class="sxs-lookup"><span data-stu-id="f6227-176">8. Problem</span></span>
<span data-ttu-id="f6227-177">Hello a következő hibaüzenet jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="f6227-177">You might see hello following error message.</span></span>

`Error: hello remote server returned an error: (407) Proxy Authentication Required.`

#### <a name="cause"></a><span data-ttu-id="f6227-178">Ok</span><span class="sxs-lookup"><span data-stu-id="f6227-178">Cause</span></span>
<span data-ttu-id="f6227-179">Ez a hiba történik, ha az átjáró egy olyan környezetben, szükséges egy HTTP-proxy tooaccess internetes erőforrásokhoz, vagy a proxy hitelesítési jelszó megváltozik, de nem frissül megfelelően a átjáróban.</span><span class="sxs-lookup"><span data-stu-id="f6227-179">This error happens when your gateway is in an environment that requires an HTTP proxy tooaccess Internet resources, or your proxy's authentication password is changed but it's not updated accordingly in your gateway.</span></span>

#### <a name="resolution"></a><span data-ttu-id="f6227-180">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="f6227-180">Resolution</span></span>
<span data-ttu-id="f6227-181">Hello hello utasításait követve [Proxy server szempontjai](#proxy-server-considerations) Ez a szakasz a következő cikket, és konfigurálja a proxybeállításokat az adatkezelési átjáró konfigurációkezelőjének.</span><span class="sxs-lookup"><span data-stu-id="f6227-181">Follow hello instructions in hello [Proxy server considerations](#proxy-server-considerations) section of this article, and configure proxy settings with Data Management Gateway Configuration Manager.</span></span>

## <a name="gateway-is-online-with-limited-functionality"></a><span data-ttu-id="f6227-182">Átjáró korlátozott funkciójú online állapotban</span><span class="sxs-lookup"><span data-stu-id="f6227-182">Gateway is online with limited functionality</span></span>
### <a name="1-problem"></a><span data-ttu-id="f6227-183">1. Probléma</span><span class="sxs-lookup"><span data-stu-id="f6227-183">1. Problem</span></span>
<span data-ttu-id="f6227-184">Hello átjáró hello állapotának online, az korlátozott funkciókkal láthatja.</span><span class="sxs-lookup"><span data-stu-id="f6227-184">You see hello status of hello gateway as online with limited functionality.</span></span>

#### <a name="cause"></a><span data-ttu-id="f6227-185">Ok</span><span class="sxs-lookup"><span data-stu-id="f6227-185">Cause</span></span>
<span data-ttu-id="f6227-186">Látni hello átjáró hello állapotának az online korlátozott funkciójú hello a következő okok valamelyike miatt:</span><span class="sxs-lookup"><span data-stu-id="f6227-186">You see hello status of hello gateway as online with limited functionality for one of hello following reasons:</span></span>

* <span data-ttu-id="f6227-187">Átjáró nem tud csatlakozni a toocloud szolgáltatás Azure Service Buson keresztül.</span><span class="sxs-lookup"><span data-stu-id="f6227-187">Gateway cannot connect toocloud service through Azure Service Bus.</span></span>
* <span data-ttu-id="f6227-188">A felhőalapú szolgáltatás nem tud kapcsolódni a toogateway Service Buson keresztül.</span><span class="sxs-lookup"><span data-stu-id="f6227-188">Cloud service cannot connect toogateway through Service Bus.</span></span>

<span data-ttu-id="f6227-189">Ha hello átjáró korlátozott funkciójú online állapotban, nem feltétlenül tudja toouse hello Data Factory másolása varázsló toocreate adatok adatcsatornákat az adatok tooor másolását a helyszíni adattárolókhoz.</span><span class="sxs-lookup"><span data-stu-id="f6227-189">When hello gateway is online with limited functionality, you might not be able toouse hello Data Factory Copy Wizard toocreate data pipelines for copying data tooor from on-premises data stores.</span></span> <span data-ttu-id="f6227-190">A probléma megoldásához használhatja Data Factory Editor hello portálon, a Visual Studio vagy Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f6227-190">As a workaround, you can use Data Factory Editor in hello portal, Visual Studio, or Azure PowerShell.</span></span>

#### <a name="resolution"></a><span data-ttu-id="f6227-191">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="f6227-191">Resolution</span></span>
<span data-ttu-id="f6227-192">A probléma feloldása (online korlátozott funkciójú) alapuló e hello átjáró nem connect toohello felhőalapú szolgáltatás, vagy más módon hello.</span><span class="sxs-lookup"><span data-stu-id="f6227-192">Resolution for this issue (online with limited functionality) is based on whether hello gateway cannot connect toohello cloud service or hello other way.</span></span> <span data-ttu-id="f6227-193">a következő szakaszok hello adja meg, hogy ezek a megoldások.</span><span class="sxs-lookup"><span data-stu-id="f6227-193">hello following sections provide these resolutions.</span></span>

### <a name="2-problem"></a><span data-ttu-id="f6227-194">2. Probléma</span><span class="sxs-lookup"><span data-stu-id="f6227-194">2. Problem</span></span>
<span data-ttu-id="f6227-195">Lásd a következő hiba hello.</span><span class="sxs-lookup"><span data-stu-id="f6227-195">You see hello following error.</span></span>

`Error: Gateway cannot connect toocloud service through service bus`

![Átjáró toocloud szolgáltatás nem tud csatlakozni.](media/data-factory-troubleshoot-gateway-issues/gateway-cannot-connect-to-cloud-service.png)

#### <a name="cause"></a><span data-ttu-id="f6227-197">Ok</span><span class="sxs-lookup"><span data-stu-id="f6227-197">Cause</span></span>
<span data-ttu-id="f6227-198">Átjáró nem tud csatlakozni a felhőalapú szolgáltatás toohello Service Buson keresztül.</span><span class="sxs-lookup"><span data-stu-id="f6227-198">Gateway cannot connect toohello cloud service through Service Bus.</span></span>

#### <a name="resolution"></a><span data-ttu-id="f6227-199">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="f6227-199">Resolution</span></span>
<span data-ttu-id="f6227-200">Kövesse a lépéseket tooget hello átjáró újra online:</span><span class="sxs-lookup"><span data-stu-id="f6227-200">Follow these steps tooget hello gateway back online:</span></span>

1. <span data-ttu-id="f6227-201">IP-cím engedélyezése a hello átjárót futtató gép és a vállalati tűzfal hello kimenő szabályok.</span><span class="sxs-lookup"><span data-stu-id="f6227-201">Allow IP address outbound rules on hello gateway machine and hello corporate firewall.</span></span> <span data-ttu-id="f6227-202">IP-címeket a Windows Eseménynapló hello található (azonosító == 401): kísérlet történt egy szoftvercsatorna tooaccess végrehajtott oly módon, a hozzáférési engedélyeket XX tiltott. A(Z) XX. A(Z) XX. XX:9350.</span><span class="sxs-lookup"><span data-stu-id="f6227-202">You can find IP addresses from hello Windows Event Log (ID == 401): An attempt was made tooaccess a socket in a way forbidden by its access permissions XX.XX.XX.XX:9350.</span></span>
* <span data-ttu-id="f6227-203">Proxybeállítások konfigurálása hello átjárón.</span><span class="sxs-lookup"><span data-stu-id="f6227-203">Configure proxy settings on hello gateway.</span></span> <span data-ttu-id="f6227-204">Lásd: hello [Proxy server szempontjai](#proxy-server-considerations) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="f6227-204">See hello [Proxy server considerations](#proxy-server-considerations) section for details.</span></span>
* <span data-ttu-id="f6227-205">Kimenő portok 5671 és 9350 – 9354-es a hello átjáró gépen és a vállalati tűzfal hello mindkét hello Windows tűzfal engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f6227-205">Enable outbound ports 5671 and 9350-9354 on both hello Windows Firewall on hello gateway machine and hello corporate firewall.</span></span> <span data-ttu-id="f6227-206">Lásd: hello [portok és a tűzfalon](#ports-and-firewall) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="f6227-206">See hello [Ports and firewall](#ports-and-firewall) section for details.</span></span> <span data-ttu-id="f6227-207">Ez a lépés nem kötelező, de a teljesítmény okokból ajánlott.</span><span class="sxs-lookup"><span data-stu-id="f6227-207">This step is optional, but we recommend it for performance consideration.</span></span>

### <a name="3-problem"></a><span data-ttu-id="f6227-208">3. Probléma</span><span class="sxs-lookup"><span data-stu-id="f6227-208">3. Problem</span></span>
<span data-ttu-id="f6227-209">Lásd a következő hiba hello.</span><span class="sxs-lookup"><span data-stu-id="f6227-209">You see hello following error.</span></span>

`Error: Cloud service cannot connect toogateway through service bus.`

#### <a name="cause"></a><span data-ttu-id="f6227-210">Ok</span><span class="sxs-lookup"><span data-stu-id="f6227-210">Cause</span></span>
<span data-ttu-id="f6227-211">Egy átmeneti hiba történt a hálózati kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="f6227-211">A transient error in network connectivity.</span></span>

#### <a name="resolution"></a><span data-ttu-id="f6227-212">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="f6227-212">Resolution</span></span>
<span data-ttu-id="f6227-213">Kövesse a lépéseket tooget hello átjáró újra online:</span><span class="sxs-lookup"><span data-stu-id="f6227-213">Follow these steps tooget hello gateway back online:</span></span>

1. <span data-ttu-id="f6227-214">Várjon néhány percig, hello kapcsolatot a rendszer automatikusan helyreállítja a eltűnt hello hiba esetén.</span><span class="sxs-lookup"><span data-stu-id="f6227-214">Wait for a couple of minutes, hello connectivity will be automatically recovered when hello error is gone.</span></span>
* <span data-ttu-id="f6227-215">Ha hello hiba továbbra is fennáll, indítsa újra a hello átjáró szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f6227-215">If hello error persists, restart hello gateway service.</span></span>

## <a name="failed-tooauthor-linked-service"></a><span data-ttu-id="f6227-216">Sikertelen tooauthor társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="f6227-216">Failed tooauthor linked service</span></span>
### <a name="problem"></a><span data-ttu-id="f6227-217">Probléma</span><span class="sxs-lookup"><span data-stu-id="f6227-217">Problem</span></span>
<span data-ttu-id="f6227-218">Ez a hiba jelenhet meg próbálja toouse hitelesítőadat-kezelő hello portál tooinput hitelesítő adatait, egy új kapcsolódó szolgáltatás, vagy egy meglévő társított szolgáltatást a hitelesítő adatok frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="f6227-218">You might see this error when you try toouse Credential Manager in hello portal tooinput credentials for a new linked service, or update credentials for an existing linked service.</span></span>

`Error: hello data store '<Server>/<Database>' cannot be reached. Check connection settings for hello data source.`

<span data-ttu-id="f6227-219">Ha ezt a hibaüzenetet látja, hello beállítások lapon az adatkezelési átjáró konfigurációkezelőjének nézhet ki például a következő képernyőkép hello.</span><span class="sxs-lookup"><span data-stu-id="f6227-219">When you see this error, hello settings page of Data Management Gateway Configuration Manager might look like hello following screenshot.</span></span>

![Adatbázis nem érhető el](media/data-factory-troubleshoot-gateway-issues/database-cannot-be-reached.png)

#### <a name="cause"></a><span data-ttu-id="f6227-221">Ok</span><span class="sxs-lookup"><span data-stu-id="f6227-221">Cause</span></span>
<span data-ttu-id="f6227-222">hello SSL-tanúsítvány előfordulhat, hogy rendelkezik elveszett hello-átjáró számítógépén.</span><span class="sxs-lookup"><span data-stu-id="f6227-222">hello SSL certificate might have been lost on hello gateway machine.</span></span> <span data-ttu-id="f6227-223">hello átjáró-számítógép hello jelenleg használt tanúsítványt az SSL-titkosítás nem tölthető be.</span><span class="sxs-lookup"><span data-stu-id="f6227-223">hello gateway computer cannot load hello certificate currently that is used for SSL encryption.</span></span> <span data-ttu-id="f6227-224">Hibaüzenet, amely a következő üzenet hasonló toohello hello eseménynaplójában is megjelenhet.</span><span class="sxs-lookup"><span data-stu-id="f6227-224">You might also see an error message in hello event log that is similar toohello following message.</span></span>

 `Unable tooget hello gateway settings from cloud service. Check hello gateway key and hello network connection. (Certificate with thumbprint cannot be loaded.)`

#### <a name="resolution"></a><span data-ttu-id="f6227-225">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="f6227-225">Resolution</span></span>
<span data-ttu-id="f6227-226">Kövesse a lépéseket toosolve hello probléma:</span><span class="sxs-lookup"><span data-stu-id="f6227-226">Follow these steps toosolve hello problem:</span></span>

1. <span data-ttu-id="f6227-227">Indítsa el az adatkezelési átjáró Konfigurációkezelőjében.</span><span class="sxs-lookup"><span data-stu-id="f6227-227">Start Data Management Gateway Configuration Manager.</span></span>
2. <span data-ttu-id="f6227-228">Váltás toohello **beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="f6227-228">Switch toohello **Settings** tab.</span></span>  
3. <span data-ttu-id="f6227-229">Kattintson a hello **módosítás** gomb toochange hello SSL-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="f6227-229">Click hello **Change** button toochange hello SSL certificate.</span></span>

   ![Módosítás tanúsítvány gombra](media/data-factory-troubleshoot-gateway-issues/change-button-ssl-certificate.png)
4. <span data-ttu-id="f6227-231">Jelöljön ki új tanúsítványt, hello SSL-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="f6227-231">Select a new certificate as hello SSL certificate.</span></span> <span data-ttu-id="f6227-232">Bármely Ön által létrehozott SSL-tanúsítvány vagy bármely szervezeti is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f6227-232">You can use any SSL certificate that is generated by you or any organization.</span></span>

   ![Adja meg a tanúsítvány](media/data-factory-troubleshoot-gateway-issues/specify-http-end-point.png)

## <a name="copy-activity-fails"></a><span data-ttu-id="f6227-234">Másolási tevékenység sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="f6227-234">Copy activity fails</span></span>
### <a name="problem"></a><span data-ttu-id="f6227-235">Probléma</span><span class="sxs-lookup"><span data-stu-id="f6227-235">Problem</span></span>
<span data-ttu-id="f6227-236">Bizonyára észrevette, hogy egy folyamat hello portálon beállítása után a következő "UserErrorFailedToConnectToSqlserver" hiba hello.</span><span class="sxs-lookup"><span data-stu-id="f6227-236">You might notice hello following "UserErrorFailedToConnectToSqlserver" failure after you set up a pipeline in hello portal.</span></span>

`Error: Copy activity encountered a user error: ErrorCode=UserErrorFailedToConnectToSqlServer,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect tooSQL Server`

#### <a name="cause"></a><span data-ttu-id="f6227-237">Ok</span><span class="sxs-lookup"><span data-stu-id="f6227-237">Cause</span></span>
<span data-ttu-id="f6227-238">Ez akkor fordulhat elő különböző okokból, és a megoldás ennek megfelelően változik.</span><span class="sxs-lookup"><span data-stu-id="f6227-238">This can happen for different reasons, and mitigation varies accordingly.</span></span>

#### <a name="resolution"></a><span data-ttu-id="f6227-239">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="f6227-239">Resolution</span></span>
<span data-ttu-id="f6227-240">Kimenő TCP-kapcsolatok engedélyezése az adatkezelési átjáró ügyféloldali hello TCP/1433-as porton keresztül tooan SQL-adatbázis csatlakoztatása előtt.</span><span class="sxs-lookup"><span data-stu-id="f6227-240">Allow outbound TCP connections over port TCP/1433 on hello Data Management Gateway client side before connecting tooan SQL database.</span></span>

<span data-ttu-id="f6227-241">Ha hello céladatbázis Azure SQL-adatbázis, ellenőrizze az SQL-kiszolgáló a tűzfal beállításai Azure is.</span><span class="sxs-lookup"><span data-stu-id="f6227-241">If hello target database is an Azure SQL database, check SQL Server firewall settings for Azure as well.</span></span>

<span data-ttu-id="f6227-242">Tekintse meg a következő szakasz tootest hello kapcsolat toohello helyszíni adattár hello.</span><span class="sxs-lookup"><span data-stu-id="f6227-242">See hello following section tootest hello connection toohello on-premises data store.</span></span>

## <a name="data-store-connection-or-driver-related-errors"></a><span data-ttu-id="f6227-243">Az adattároló csatlakozási vagy kapcsolatos hibák</span><span class="sxs-lookup"><span data-stu-id="f6227-243">Data store connection or driver-related errors</span></span>
<span data-ttu-id="f6227-244">Ha adatok tárolásához a kapcsolódáshoz vagy az illesztőprogram-kapcsolatos hibákat, végezze el a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="f6227-244">If you see data store connection or driver-related errors, complete hello following steps:</span></span>

1. <span data-ttu-id="f6227-245">Indítsa el az adatkezelési átjáró konfigurációkezelőjének hello-átjáró számítógépén.</span><span class="sxs-lookup"><span data-stu-id="f6227-245">Start Data Management Gateway Configuration Manager on hello gateway machine.</span></span>
2. <span data-ttu-id="f6227-246">Váltás toohello **diagnosztika** fülre.</span><span class="sxs-lookup"><span data-stu-id="f6227-246">Switch toohello **Diagnostics** tab.</span></span>
3. <span data-ttu-id="f6227-247">A **kapcsolat tesztelése**, hello átjáró csoport értékek hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="f6227-247">In **Test Connection**, add hello gateway group values.</span></span>
4. <span data-ttu-id="f6227-248">Kattintson a **teszt** toosee toohello tudja csatlakoztatni a helyszíni adatforrás hello átjáró gépről hello kapcsolati adatokat és hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="f6227-248">Click **Test** toosee if you can connect toohello on-premises data source from hello gateway machine by using hello connection information and credentials.</span></span> <span data-ttu-id="f6227-249">Hello kapcsolat tesztelése az illesztőprogram telepítése után továbbra is sikertelen, ha újraindítás hello átjáró azt toopick hello legújabb módosítás mentése.</span><span class="sxs-lookup"><span data-stu-id="f6227-249">If hello test connection still fails after you install a driver, restart hello gateway for it toopick up hello latest change.</span></span>

![Kapcsolat tesztelése a Diagnosztika lap](media/data-factory-troubleshoot-gateway-issues/test-connection-in-diagnostics-tab.png)

## <a name="gateway-logs"></a><span data-ttu-id="f6227-251">Átjáró naplói</span><span class="sxs-lookup"><span data-stu-id="f6227-251">Gateway logs</span></span>
### <a name="send-gateway-logs-toomicrosoft"></a><span data-ttu-id="f6227-252">Átjáró naplók tooMicrosoft küldése</span><span class="sxs-lookup"><span data-stu-id="f6227-252">Send gateway logs tooMicrosoft</span></span>
<span data-ttu-id="f6227-253">Amikor kapcsolatba lép a Microsoft Support tooget segítséget átjáró problémák elhárításához, megadását tooshare az átjáró naplói.</span><span class="sxs-lookup"><span data-stu-id="f6227-253">When you contact Microsoft Support tooget help with troubleshooting gateway issues, you might be asked tooshare your gateway logs.</span></span> <span data-ttu-id="f6227-254">Hello kiadás hello átjáró, a szükséges átjáró naplók és a két gombra történő kattintás az adatkezelési átjáró konfigurációkezelőjének is megoszthatja.</span><span class="sxs-lookup"><span data-stu-id="f6227-254">With hello release of hello gateway, you can share required gateway logs with two button clicks in Data Management Gateway Configuration Manager.</span></span>    

1. <span data-ttu-id="f6227-255">Váltás toohello **diagnosztika** az adatkezelési átjáró konfigurációkezelőjének fülre.</span><span class="sxs-lookup"><span data-stu-id="f6227-255">Switch toohello **Diagnostics** tab in Data Management Gateway Configuration Manager.</span></span>

    ![Felügyeleti átjáró Diagnostics lapon](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-diagnostics-tab.png)
2. <span data-ttu-id="f6227-257">Kattintson a **naplók küldése** toosee hello a következő párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="f6227-257">Click **Send Logs** toosee hello following dialog box.</span></span>

    ![Felügyeleti átjáró küldése adatnaplók](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-dialog.png)
3. <span data-ttu-id="f6227-259">(Választható) Kattintson a **naplók megtekintése** tooreview hello eseménynaplóban naplózza.</span><span class="sxs-lookup"><span data-stu-id="f6227-259">(Optional) Click **view logs** tooreview logs in hello event viewer.</span></span>
4. <span data-ttu-id="f6227-260">(Választható) Kattintson a **adatvédelmi** tooreview Microsoft web services adatvédelmi nyilatkozatát.</span><span class="sxs-lookup"><span data-stu-id="f6227-260">(Optional) Click **privacy** tooreview Microsoft web services privacy statement.</span></span>
5. <span data-ttu-id="f6227-261">Ha elégedett a tooupload kapcsolatos esetén, kattintson a **naplók küldése** tooactually hello elmúlt hét napban tooMicrosoft hibaelhárítási hello naplókat küldeni.</span><span class="sxs-lookup"><span data-stu-id="f6227-261">When you are satisfied with what you are about tooupload, click **Send Logs** tooactually send hello logs from hello last seven days tooMicrosoft for troubleshooting.</span></span> <span data-ttu-id="f6227-262">Ahogy az alábbi képernyőfelvétel a hello hello küldési-naplók művelet hello állapotának kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="f6227-262">You should see hello status of hello send-logs operation as shown in hello following screenshot.</span></span>

    ![Adatok felügyeleti átjáró küldése naplózza állapota](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-status.png)
6. <span data-ttu-id="f6227-264">Hello művelet befejezése után, párbeszédpanel jelenik meg a látható módon a következő képernyőkép hello.</span><span class="sxs-lookup"><span data-stu-id="f6227-264">After hello operation is complete, you see a dialog box as shown in hello following screenshot.</span></span>

    ![Adatok felügyeleti átjáró küldése naplózza állapota](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-result.png)
7. <span data-ttu-id="f6227-266">Mentse a hello **azonosítója** és ossza meg a Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="f6227-266">Save hello **Report ID** and share it with Microsoft Support.</span></span> <span data-ttu-id="f6227-267">hello azonosítója használt toolocate hello átjáró naplók hibaelhárítási feltöltött.</span><span class="sxs-lookup"><span data-stu-id="f6227-267">hello report ID is used toolocate hello gateway logs that you uploaded for troubleshooting.</span></span>  <span data-ttu-id="f6227-268">hello azonosítója hello eseménynaplójában is menti.</span><span class="sxs-lookup"><span data-stu-id="f6227-268">hello report ID is also saved in hello event viewer.</span></span>  <span data-ttu-id="f6227-269">Hello eseményazonosító "25" megtekintésével megtalálja, és ellenőrizze a hello dátuma és időpontja.</span><span class="sxs-lookup"><span data-stu-id="f6227-269">You can find it by looking at hello event ID “25”, and check hello date and time.</span></span>

    ![Adatok felügyeleti átjáró küldése naplózza az azonosítója](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a><span data-ttu-id="f6227-271">Az átjáró állomás számítógépén archív-átjáró naplói</span><span class="sxs-lookup"><span data-stu-id="f6227-271">Archive gateway logs on gateway host machine</span></span>
<span data-ttu-id="f6227-272">Léteznek olyan forgatókönyvek, ahol átjáró problémák vannak, és közvetlenül nem osztható meg átjáró naplói:</span><span class="sxs-lookup"><span data-stu-id="f6227-272">There are some scenarios where you have gateway issues and you cannot share gateway logs directly:</span></span>

* <span data-ttu-id="f6227-273">Manuálisan telepítse hello átjárót, és hello átjáró regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="f6227-273">You manually install hello gateway and register hello gateway.</span></span>
* <span data-ttu-id="f6227-274">Az adatkezelési átjáró konfigurációkezelőjének újragenerált kulccsal tooregister hello átjáró meg.</span><span class="sxs-lookup"><span data-stu-id="f6227-274">You try tooregister hello gateway with a regenerated key in Data Management Gateway Configuration Manager.</span></span>
* <span data-ttu-id="f6227-275">Toosend naplók meg, és nem lehet csatlakozni a hello átjáró gazdaszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f6227-275">You try toosend logs and hello gateway host service cannot be connected.</span></span>

<span data-ttu-id="f6227-276">Ezek a forgatókönyvek az átjáró naplói elmentse egy zip-fájlt, és ossza meg, amikor a Microsoft támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="f6227-276">For these scenarios, you can save gateway logs as a zip file and share it when you contact Microsoft support.</span></span> <span data-ttu-id="f6227-277">Például ha arra vonatkozó hibaüzenetet kap, amíg hello átjáró mint regisztrálnia jelenik meg a következő képernyőkép hello.</span><span class="sxs-lookup"><span data-stu-id="f6227-277">For example, if you receive an error while you register hello gateway as shown in hello following screenshot.</span></span>   

![Felügyeleti átjáró regisztrációs hiba](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-registration-error.png)

<span data-ttu-id="f6227-279">Kattintson a hello **átjáró naplók archiválása** tooarchive hivatkozásra, és mentse a naplókat, és majd hello zip-fájl megosztása a Microsoft támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="f6227-279">Click hello **Archive gateway logs** link tooarchive and save logs, and then share hello zip file with Microsoft support.</span></span>

![Felügyeleti átjáró archív adatnaplók](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-archive-logs.png)

### <a name="locate-gateway-logs"></a><span data-ttu-id="f6227-281">Keresse meg az átjáró naplói</span><span class="sxs-lookup"><span data-stu-id="f6227-281">Locate gateway logs</span></span>
<span data-ttu-id="f6227-282">Részletes átjáró napló információt hello Windows eseménynaplóiban talál.</span><span class="sxs-lookup"><span data-stu-id="f6227-282">You can find detailed gateway log information in hello Windows event logs.</span></span>

1. <span data-ttu-id="f6227-283">A Windows indítása **Eseménynapló**.</span><span class="sxs-lookup"><span data-stu-id="f6227-283">Start Windows **Event Viewer**.</span></span>
2. <span data-ttu-id="f6227-284">Keresse meg a naplókat a hello **alkalmazás- és szolgáltatásnaplók** > **az adatkezelési átjáró** mappát.</span><span class="sxs-lookup"><span data-stu-id="f6227-284">Locate logs in hello **Application and Services Logs** > **Data Management Gateway** folder.</span></span>

 <span data-ttu-id="f6227-285">Átjáró kapcsolatos problémák elhárításakor még szintű hibaesemények hello eseménynaplójában keresse meg.</span><span class="sxs-lookup"><span data-stu-id="f6227-285">When you're troubleshooting gateway-related issues, look for error level events in hello event viewer.</span></span>

![Az adatkezelési átjáró naplózza az eseménynaplóban](media/data-factory-troubleshoot-gateway-issues/gateway-logs-event-viewer.png)
