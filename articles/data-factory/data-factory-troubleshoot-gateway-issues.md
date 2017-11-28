---
title: "Az adatkezelési átjáró elhárítása |} Microsoft Docs"
description: "Tippek az adatkezelési átjáró kapcsolatos problémák elhárítása érdekében."
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
ms.openlocfilehash: b8e50a4e3c0b9c535ccc2344ff22261a356d5acc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshoot-issues-with-using-data-management-gateway"></a><span data-ttu-id="61933-103">Az adatkezelési átjáró használata közben felmerülő hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="61933-103">Troubleshoot issues with using Data Management Gateway</span></span>
<span data-ttu-id="61933-104">Ez a cikk tájékoztatást nyújt az adatkezelési átjáró használatával kapcsolatos hibák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="61933-104">This article provides information on troubleshooting issues with using Data Management Gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="61933-105">Tekintse meg a [az adatkezelési átjáró](data-factory-data-management-gateway.md) cikk az átjáró kapcsolatos részletes információkat.</span><span class="sxs-lookup"><span data-stu-id="61933-105">See the [Data Management Gateway](data-factory-data-management-gateway.md) article for detailed information about the gateway.</span></span> <span data-ttu-id="61933-106">Tekintse meg a [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) áthelyezése adatok egy helyi SQL Server-adatbázis a Microsoft Azure Blob storage az átjáró használatával kapcsolatos általános bemutatóért cikkében.</span><span class="sxs-lookup"><span data-stu-id="61933-106">See the [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for a walkthrough of moving data from an on-premises SQL Server database to Microsoft Azure Blob storage by using the gateway.</span></span>
>
>

## <a name="failed-to-install-or-register-gateway"></a><span data-ttu-id="61933-107">Nem sikerült telepíteni vagy az átjáró regisztrálása</span><span class="sxs-lookup"><span data-stu-id="61933-107">Failed to install or register gateway</span></span>
### <a name="1-problem"></a><span data-ttu-id="61933-108">1. Probléma</span><span class="sxs-lookup"><span data-stu-id="61933-108">1. Problem</span></span>
<span data-ttu-id="61933-109">Megjelenik a hibaüzenet, amikor telepítése és regisztrálása az átjáró, az átjáró telepítési fájl letöltése során.</span><span class="sxs-lookup"><span data-stu-id="61933-109">You see this error message when installing and registering a gateway, specifically, while downloading the gateway installation file.</span></span>

`Unable to connect to the remote server". Please check your local settings (Error Code: 10003).`

#### <a name="cause"></a><span data-ttu-id="61933-110">Ok</span><span class="sxs-lookup"><span data-stu-id="61933-110">Cause</span></span>
<span data-ttu-id="61933-111">A gép, amelyen az átjáró telepítéséhez kívánt letöltőközpontból az átjáró legújabb telepítési fájlját a hálózati probléma miatt nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="61933-111">The machine on which you are trying to install the gateway has failed to download the latest gateway installation file from the download center due to a network issue.</span></span>

#### <a name="resolution"></a><span data-ttu-id="61933-112">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="61933-112">Resolution</span></span>
<span data-ttu-id="61933-113">Ellenőrizze a tűzfal proxykiszolgáló beállításait megtekintéséhez, hogy a beállítások tiltsák le a hálózati kapcsolat a számítógépről a [letöltőközpontból](https://download.microsoft.com/), frissítse a beállításokat, és ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="61933-113">Check your firewall proxy server settings to see whether the settings block the network connection from the computer to the [download center](https://download.microsoft.com/), and update the settings accordingly.</span></span>

<span data-ttu-id="61933-114">Azt is megteheti, letöltheti a legújabb átjárót telepítőfájlja a [letöltőközpontból](https://www.microsoft.com/download/details.aspx?id=39717) más számítógépeken, amelyek hozzáférhetnek a letöltőközpontból.</span><span class="sxs-lookup"><span data-stu-id="61933-114">Alternatively, you can download the installation file for the latest gateway from the [download center](https://www.microsoft.com/download/details.aspx?id=39717) on other machines that can access the download center.</span></span> <span data-ttu-id="61933-115">A telepítőfájl másolja az átjáró gazdaszámítógépet, majd futtassa azt manuálisan kell telepíteni, és frissítheti az átjárót.</span><span class="sxs-lookup"><span data-stu-id="61933-115">You can then copy the installer file to the gateway host computer and run it manually to install and update the gateway.</span></span>

### <a name="2-problem"></a><span data-ttu-id="61933-116">2. Probléma</span><span class="sxs-lookup"><span data-stu-id="61933-116">2. Problem</span></span>
<span data-ttu-id="61933-117">Ezt a hibaüzenetet látja, ha telepít egy átjárót kattintva végrehajtani kívánt **telepíthető közvetlenül a számítógépen** az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="61933-117">You see this error when you're attempting to install a gateway by clicking **install directly on this computer** in the Azure portal.</span></span>

`Error:  Abort installing a new gateway on this computer because this computer has an existing installed gateway and a computer without any installed gateway is required for installing a new gateway.`  

#### <a name="cause"></a><span data-ttu-id="61933-118">Ok</span><span class="sxs-lookup"><span data-stu-id="61933-118">Cause</span></span>
<span data-ttu-id="61933-119">Átjáró már telepítve van a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="61933-119">A gateway is already installed on the machine.</span></span>

#### <a name="resolution"></a><span data-ttu-id="61933-120">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="61933-120">Resolution</span></span>
<span data-ttu-id="61933-121">Távolítsa el a gépen a jelenlegi átjárót, majd kattintson a **telepíthető közvetlenül a számítógépen** újra hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="61933-121">Uninstall the existing gateway on the machine and click the **install directly on this computer** link again.</span></span>

### <a name="3-problem"></a><span data-ttu-id="61933-122">3. Probléma</span><span class="sxs-lookup"><span data-stu-id="61933-122">3. Problem</span></span>
<span data-ttu-id="61933-123">Ez a hiba jelenhet meg, amikor egy új átjáró regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="61933-123">You might see this error when registering a new gateway.</span></span>

`Error: The gateway has encountered an error during registration.`

#### <a name="cause"></a><span data-ttu-id="61933-124">Ok</span><span class="sxs-lookup"><span data-stu-id="61933-124">Cause</span></span>
<span data-ttu-id="61933-125">Ez az üzenet a következő okok valamelyike láthatja:</span><span class="sxs-lookup"><span data-stu-id="61933-125">You might see this message for one of the following reasons:</span></span>

* <span data-ttu-id="61933-126">Az átjáró kulcsát formátuma érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="61933-126">The format of the gateway key is invalid.</span></span>
* <span data-ttu-id="61933-127">Az átjáró kulcsát érvénytelenné vált.</span><span class="sxs-lookup"><span data-stu-id="61933-127">The gateway key has been invalidated.</span></span>
* <span data-ttu-id="61933-128">Az átjáró kulcsát újra lett létrehozza a rendszer a portálról.</span><span class="sxs-lookup"><span data-stu-id="61933-128">The gateway key has been regenerated from the portal.</span></span>  

#### <a name="resolution"></a><span data-ttu-id="61933-129">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="61933-129">Resolution</span></span>
<span data-ttu-id="61933-130">Győződjön meg arról, hogy használ-e a megfelelő átjárókulcsot, amit a portálon.</span><span class="sxs-lookup"><span data-stu-id="61933-130">Verify whether you are using the right gateway key from the portal.</span></span> <span data-ttu-id="61933-131">Ha szükséges, a kulcs újragenerálása és a kulcs segítségével regisztrálja az átjárót.</span><span class="sxs-lookup"><span data-stu-id="61933-131">If needed, regenerate a key and use the key to register the gateway.</span></span>

### <a name="4-problem"></a><span data-ttu-id="61933-132">4. Probléma</span><span class="sxs-lookup"><span data-stu-id="61933-132">4. Problem</span></span>
<span data-ttu-id="61933-133">Átjáró regisztrálásakor még a következő hibaüzenet jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="61933-133">You might see the following error message when you're registering a gateway.</span></span>

`Error: The content or format of the gateway key "{gatewayKey}" is invalid, please go to azure portal to create one new gateway or regenerate the gateway key.`



![Tartalom és a kulcs formátuma érvénytelen](media/data-factory-troubleshoot-gateway-issues/invalid-format-gateway-key.png)

#### <a name="cause"></a><span data-ttu-id="61933-135">Ok</span><span class="sxs-lookup"><span data-stu-id="61933-135">Cause</span></span>
<span data-ttu-id="61933-136">A tartalom vagy a bemeneti átjárókulcs formátuma nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="61933-136">The content or format of the input gateway key is incorrect.</span></span> <span data-ttu-id="61933-137">Egyik oka lehet, hogy a kulcsot csak egy részét átmásolva a portálon, vagy érvénytelen kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="61933-137">One of the reasons can be that you copied only a portion of the key from the portal or you're using an invalid key.</span></span>

#### <a name="resolution"></a><span data-ttu-id="61933-138">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="61933-138">Resolution</span></span>
<span data-ttu-id="61933-139">Hozzon létre egy átjáró kulcsot a portálon, és használja a Másolás gombra a teljes kulcsot másolja.</span><span class="sxs-lookup"><span data-stu-id="61933-139">Generate a gateway key in the portal, and use the copy button to copy the whole key.</span></span> <span data-ttu-id="61933-140">Illessze be az ablakot, hogy regisztrálja az átjárót majd.</span><span class="sxs-lookup"><span data-stu-id="61933-140">Then paste it in this window to register the gateway.</span></span>

### <a name="5-problem"></a><span data-ttu-id="61933-141">5. Probléma</span><span class="sxs-lookup"><span data-stu-id="61933-141">5. Problem</span></span>
<span data-ttu-id="61933-142">Átjáró regisztrálásakor még a következő hibaüzenet jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="61933-142">You might see the following error message when you're registering a gateway.</span></span>

`Error: The gateway key is invalid or empty. Specify a valid gateway key from the portal.`

![Átjáró kulcsa érvénytelen vagy üres:](media/data-factory-troubleshoot-gateway-issues/gateway-key-is-invalid-or-empty.png)

#### <a name="cause"></a><span data-ttu-id="61933-144">Ok</span><span class="sxs-lookup"><span data-stu-id="61933-144">Cause</span></span>
<span data-ttu-id="61933-145">Az átjáró kulcsát újra lett létrehozva. vagy az átjáró törölve lett az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="61933-145">The gateway key has been regenerated or the gateway has been deleted in the Azure portal.</span></span> <span data-ttu-id="61933-146">Akkor is előfordulhat, ha az adatkezelési átjáró beállítása nincs legújabb.</span><span class="sxs-lookup"><span data-stu-id="61933-146">It can also happen if the Data Management Gateway setup is not latest.</span></span>

#### <a name="resolution"></a><span data-ttu-id="61933-147">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="61933-147">Resolution</span></span>
<span data-ttu-id="61933-148">Ellenőrizze, hogy ha a telepítő az adatkezelési átjáró legújabb verziója, a Microsoft a legújabb verzió található [letöltőközpontból](https://go.microsoft.com/fwlink/p/?LinkId=271260).</span><span class="sxs-lookup"><span data-stu-id="61933-148">Check if the Data Management Gateway setup is the latest version, you can find the latest version on the Microsoft [download center](https://go.microsoft.com/fwlink/p/?LinkId=271260).</span></span>

<span data-ttu-id="61933-149">Ha a beállítás jelenlegi / legújabb és átjáró még létezik a portálon, újragenerálja az átjáró kulcsát az Azure-portálon használ a Másolás gombra a teljes kulcsot másolja, és majd illessze be az ablakot, hogy regisztrálja az átjárót.</span><span class="sxs-lookup"><span data-stu-id="61933-149">If setup is current/ latest and gateway still exists on Portal, regenerate the gateway key in the Azure portal, and use the copy button to copy the whole key, and then paste it in this window to register the gateway.</span></span> <span data-ttu-id="61933-150">Ellenkező esetben hozza létre újra az átjárót, és kezdje újra a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="61933-150">Otherwise, recreate the gateway and start over.</span></span>

### <a name="6-problem"></a><span data-ttu-id="61933-151">6. Probléma</span><span class="sxs-lookup"><span data-stu-id="61933-151">6. Problem</span></span>
<span data-ttu-id="61933-152">Átjáró regisztrálásakor még a következő hibaüzenet jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="61933-152">You might see the following error message when you're registering a gateway.</span></span>

`Error: Gateway has been online for a while, then shows “Gateway is not registered” with the status “Gateway key is invalid”`

![Átjáró kulcsa érvénytelen vagy üres:](media/data-factory-troubleshoot-gateway-issues/gateway-not-registered-key-invalid.png)

#### <a name="cause"></a><span data-ttu-id="61933-154">Ok</span><span class="sxs-lookup"><span data-stu-id="61933-154">Cause</span></span>
<span data-ttu-id="61933-155">Ez a hiba akkor fordulhat elő, mert az átjáró törölve lett, vagy a kapcsolódó átjárókulcs újra lett létrehozva..</span><span class="sxs-lookup"><span data-stu-id="61933-155">This error might happen because either the gateway has been deleted or the associated gateway key has been regenerated.</span></span>

#### <a name="resolution"></a><span data-ttu-id="61933-156">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="61933-156">Resolution</span></span>
<span data-ttu-id="61933-157">Ha az átjáró törölve lett, hozza létre újból az átjárót a portál, kattintson a **regisztrálása**, a kulcs másolása a portálról, illessze be és próbálja meg regisztrálni az átjárót.</span><span class="sxs-lookup"><span data-stu-id="61933-157">If the gateway has been deleted, re-create the gateway from the portal, click **Register**, copy the key from the portal, paste it, and try to register the gateway.</span></span>

<span data-ttu-id="61933-158">Ha az átjáró még létezik, de a kulcsot újra lett létrehozva., az új kulcs segítségével regisztrálja az átjárót.</span><span class="sxs-lookup"><span data-stu-id="61933-158">If the gateway still exists but its key has been regenerated, use the new key to register the gateway.</span></span> <span data-ttu-id="61933-159">Ha még nem rendelkezik a kulcsot, újragenerálja a kulcsot újra a portálról.</span><span class="sxs-lookup"><span data-stu-id="61933-159">If you don’t have the key, regenerate the key again from the portal.</span></span>

### <a name="7-problem"></a><span data-ttu-id="61933-160">7. Probléma</span><span class="sxs-lookup"><span data-stu-id="61933-160">7. Problem</span></span>
<span data-ttu-id="61933-161">Átjáró regisztrálásakor van szükség lehet a tanúsítvány elérési útja és a jelszó megadását.</span><span class="sxs-lookup"><span data-stu-id="61933-161">When you're registering a gateway, you might need to enter path and password for a certificate.</span></span>

![Adja meg a tanúsítvány](media/data-factory-troubleshoot-gateway-issues/specify-certificate.png)

#### <a name="cause"></a><span data-ttu-id="61933-163">Ok</span><span class="sxs-lookup"><span data-stu-id="61933-163">Cause</span></span>
<span data-ttu-id="61933-164">Az átjáró más gépeken előtt regisztrálva van.</span><span class="sxs-lookup"><span data-stu-id="61933-164">The gateway has been registered on other machines before.</span></span> <span data-ttu-id="61933-165">Átjáró, a kezdeti regisztráció során egy titkosítási tanúsítvány nincs hozzárendelve az átjárót.</span><span class="sxs-lookup"><span data-stu-id="61933-165">During the initial registration of a gateway, an encryption certificate has been associated with the gateway.</span></span> <span data-ttu-id="61933-166">A tanúsítvány lehet önálló az átjáró által generált, vagy a felhasználó által megadott.</span><span class="sxs-lookup"><span data-stu-id="61933-166">The certificate can either be self-generated by the gateway or provided by the user.</span></span>  <span data-ttu-id="61933-167">Ezzel a tanúsítvánnyal az adattár (társított szolgáltatás) hitelesítő adatok titkosításához.</span><span class="sxs-lookup"><span data-stu-id="61933-167">This certificate is used to encrypt credentials of the data store (linked service).</span></span>  

![Tanúsítvány exportálása](media/data-factory-troubleshoot-gateway-issues/export-certificate.png)

<span data-ttu-id="61933-169">Az átjáró másik gazdaszámítógépet visszaállításakor a regisztrációs varázsló megkérdezi, a tanúsítvány visszafejtése a korábban a tanúsítvánnyal titkosított hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="61933-169">When restoring the gateway on a different host machine, the registration wizard asks for this certificate to decrypt credentials previously encrypted with this certificate.</span></span>  <span data-ttu-id="61933-170">Ez a tanúsítvány nélkül nem tudja visszafejteni a hitelesítő adatokat az új átjáró, és az új átjáró társított későbbi másolási tevékenység végrehajtások meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="61933-170">Without this certificate, the credentials cannot be decrypted by the new gateway and subsequent copy activity executions associated with this new gateway will fail.</span></span>  

#### <a name="resolution"></a><span data-ttu-id="61933-171">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="61933-171">Resolution</span></span>
<span data-ttu-id="61933-172">Ha exportálta a hitelesítési tanúsítványt az eredeti átjáró gépen használatával a **exportálása** gombra a **beállítások** az adatkezelési átjáró konfigurációkezelőjének lapon, a tanúsítvány használatára itt.</span><span class="sxs-lookup"><span data-stu-id="61933-172">If you have exported the credential certificate from the original gateway machine by using the **Export** button on the **Settings** tab in Data Management Gateway Configuration Manager, use the certificate here.</span></span>

<span data-ttu-id="61933-173">Ebben a szakaszban nem hagyható ki, amikor egy átjáró helyreállítása.</span><span class="sxs-lookup"><span data-stu-id="61933-173">You cannot skip this stage when recovering a gateway.</span></span> <span data-ttu-id="61933-174">Ha a tanúsítvány hiányzik, törölje az átjárót a portálról, és hozza létre az új átjáró szüksége.</span><span class="sxs-lookup"><span data-stu-id="61933-174">If the certificate is missing, you need to delete the gateway from the portal and re-create a new gateway.</span></span>  <span data-ttu-id="61933-175">Emellett frissítse az átjáró által ismét be kell írni a hitelesítő adataik kapcsolódó összes társított szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="61933-175">In addition, update all linked services that are related to the gateway by reentering their credentials.</span></span>

### <a name="8-problem"></a><span data-ttu-id="61933-176">8. Probléma</span><span class="sxs-lookup"><span data-stu-id="61933-176">8. Problem</span></span>
<span data-ttu-id="61933-177">A következő hibaüzenet jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="61933-177">You might see the following error message.</span></span>

`Error: The remote server returned an error: (407) Proxy Authentication Required.`

#### <a name="cause"></a><span data-ttu-id="61933-178">Ok</span><span class="sxs-lookup"><span data-stu-id="61933-178">Cause</span></span>
<span data-ttu-id="61933-179">Ez a hiba történik, ha az átjáró egy olyan környezetben, szükséges a HTTP-proxy elérni az internetes erőforrásokhoz, vagy a proxy hitelesítési jelszó megváltozik, de nem frissül megfelelően a átjáróban.</span><span class="sxs-lookup"><span data-stu-id="61933-179">This error happens when your gateway is in an environment that requires an HTTP proxy to access Internet resources, or your proxy's authentication password is changed but it's not updated accordingly in your gateway.</span></span>

#### <a name="resolution"></a><span data-ttu-id="61933-180">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="61933-180">Resolution</span></span>
<span data-ttu-id="61933-181">Kövesse az utasításokat a [Proxy server szempontjai](#proxy-server-considerations) Ez a szakasz a következő cikket, és konfigurálja a proxybeállításokat az adatkezelési átjáró konfigurációkezelőjének.</span><span class="sxs-lookup"><span data-stu-id="61933-181">Follow the instructions in the [Proxy server considerations](#proxy-server-considerations) section of this article, and configure proxy settings with Data Management Gateway Configuration Manager.</span></span>

## <a name="gateway-is-online-with-limited-functionality"></a><span data-ttu-id="61933-182">Átjáró korlátozott funkciójú online állapotban</span><span class="sxs-lookup"><span data-stu-id="61933-182">Gateway is online with limited functionality</span></span>
### <a name="1-problem"></a><span data-ttu-id="61933-183">1. Probléma</span><span class="sxs-lookup"><span data-stu-id="61933-183">1. Problem</span></span>
<span data-ttu-id="61933-184">Az átjáró állapotának online, az korlátozott funkciókkal láthatja.</span><span class="sxs-lookup"><span data-stu-id="61933-184">You see the status of the gateway as online with limited functionality.</span></span>

#### <a name="cause"></a><span data-ttu-id="61933-185">Ok</span><span class="sxs-lookup"><span data-stu-id="61933-185">Cause</span></span>
<span data-ttu-id="61933-186">Megjelenik az átjáró állapotának online korlátozott szolgáltatásokkal a következő okok valamelyike:</span><span class="sxs-lookup"><span data-stu-id="61933-186">You see the status of the gateway as online with limited functionality for one of the following reasons:</span></span>

* <span data-ttu-id="61933-187">Átjáró nem tud kapcsolódni a felhőalapú szolgáltatás Azure Service Buson keresztül.</span><span class="sxs-lookup"><span data-stu-id="61933-187">Gateway cannot connect to cloud service through Azure Service Bus.</span></span>
* <span data-ttu-id="61933-188">A felhőalapú szolgáltatás nem tud kapcsolódni a Service Buson keresztül átjáró.</span><span class="sxs-lookup"><span data-stu-id="61933-188">Cloud service cannot connect to gateway through Service Bus.</span></span>

<span data-ttu-id="61933-189">Ha az átjáró online korlátozott szolgáltatásokkal, nem feltétlenül a Data Factory másolása varázsló használatával hozzon létre az adatok másolása, vagy a helyszíni adattárolókhoz adatok folyamatok.</span><span class="sxs-lookup"><span data-stu-id="61933-189">When the gateway is online with limited functionality, you might not be able to use the Data Factory Copy Wizard to create data pipelines for copying data to or from on-premises data stores.</span></span> <span data-ttu-id="61933-190">A probléma megoldásához használhatja Data Factory Editor a portálon, a Visual Studio vagy az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="61933-190">As a workaround, you can use Data Factory Editor in the portal, Visual Studio, or Azure PowerShell.</span></span>

#### <a name="resolution"></a><span data-ttu-id="61933-191">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="61933-191">Resolution</span></span>
<span data-ttu-id="61933-192">A probléma feloldása (online korlátozott funkciójú) azon alapul, hogy az átjáró nem tud csatlakozni a felhőalapú szolgáltatás vagy a más módon.</span><span class="sxs-lookup"><span data-stu-id="61933-192">Resolution for this issue (online with limited functionality) is based on whether the gateway cannot connect to the cloud service or the other way.</span></span> <span data-ttu-id="61933-193">A következő szakaszokban ezek a megoldások.</span><span class="sxs-lookup"><span data-stu-id="61933-193">The following sections provide these resolutions.</span></span>

### <a name="2-problem"></a><span data-ttu-id="61933-194">2. Probléma</span><span class="sxs-lookup"><span data-stu-id="61933-194">2. Problem</span></span>
<span data-ttu-id="61933-195">Az alábbi hibaüzenet látható.</span><span class="sxs-lookup"><span data-stu-id="61933-195">You see the following error.</span></span>

`Error: Gateway cannot connect to cloud service through service bus`

![Átjáró felhőalapú szolgáltatás nem tud kapcsolódni.](media/data-factory-troubleshoot-gateway-issues/gateway-cannot-connect-to-cloud-service.png)

#### <a name="cause"></a><span data-ttu-id="61933-197">Ok</span><span class="sxs-lookup"><span data-stu-id="61933-197">Cause</span></span>
<span data-ttu-id="61933-198">Átjáró nem tud kapcsolódni a Service Buson keresztül a felhőalapú szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="61933-198">Gateway cannot connect to the cloud service through Service Bus.</span></span>

#### <a name="resolution"></a><span data-ttu-id="61933-199">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="61933-199">Resolution</span></span>
<span data-ttu-id="61933-200">Kövesse az alábbi lépéseket követve az átjáró online:</span><span class="sxs-lookup"><span data-stu-id="61933-200">Follow these steps to get the gateway back online:</span></span>

1. <span data-ttu-id="61933-201">IP-címet az átjáró számítógépe és a vállalati tűzfalon kimenő szabályok engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="61933-201">Allow IP address outbound rules on the gateway machine and the corporate firewall.</span></span> <span data-ttu-id="61933-202">Található IP-címek a Windows Eseménynapló (ID == 401): kísérlet történt olyan módon, a hozzáférési engedélyeket XX tiltott hozzáférés. A(Z) XX. A(Z) XX. XX:9350.</span><span class="sxs-lookup"><span data-stu-id="61933-202">You can find IP addresses from the Windows Event Log (ID == 401): An attempt was made to access a socket in a way forbidden by its access permissions XX.XX.XX.XX:9350.</span></span>
* <span data-ttu-id="61933-203">Proxybeállítások konfigurálása az átjárón.</span><span class="sxs-lookup"><span data-stu-id="61933-203">Configure proxy settings on the gateway.</span></span> <span data-ttu-id="61933-204">Tekintse meg a [Proxy server szempontjai](#proxy-server-considerations) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="61933-204">See the [Proxy server considerations](#proxy-server-considerations) section for details.</span></span>
* <span data-ttu-id="61933-205">Engedélyezze a kimenő portok 5671 és 9350 – 9354-es mindkét a Windows tűzfal az átjáró gépen és a vállalati tűzfalon.</span><span class="sxs-lookup"><span data-stu-id="61933-205">Enable outbound ports 5671 and 9350-9354 on both the Windows Firewall on the gateway machine and the corporate firewall.</span></span> <span data-ttu-id="61933-206">Tekintse meg a [portok és a tűzfalon](#ports-and-firewall) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="61933-206">See the [Ports and firewall](#ports-and-firewall) section for details.</span></span> <span data-ttu-id="61933-207">Ez a lépés nem kötelező, de a teljesítmény okokból ajánlott.</span><span class="sxs-lookup"><span data-stu-id="61933-207">This step is optional, but we recommend it for performance consideration.</span></span>

### <a name="3-problem"></a><span data-ttu-id="61933-208">3. Probléma</span><span class="sxs-lookup"><span data-stu-id="61933-208">3. Problem</span></span>
<span data-ttu-id="61933-209">Az alábbi hibaüzenet látható.</span><span class="sxs-lookup"><span data-stu-id="61933-209">You see the following error.</span></span>

`Error: Cloud service cannot connect to gateway through service bus.`

#### <a name="cause"></a><span data-ttu-id="61933-210">Ok</span><span class="sxs-lookup"><span data-stu-id="61933-210">Cause</span></span>
<span data-ttu-id="61933-211">Egy átmeneti hiba történt a hálózati kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="61933-211">A transient error in network connectivity.</span></span>

#### <a name="resolution"></a><span data-ttu-id="61933-212">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="61933-212">Resolution</span></span>
<span data-ttu-id="61933-213">Kövesse az alábbi lépéseket követve az átjáró online:</span><span class="sxs-lookup"><span data-stu-id="61933-213">Follow these steps to get the gateway back online:</span></span>

1. <span data-ttu-id="61933-214">Várjon néhány percig, a kapcsolatot a rendszer automatikusan helyreállítja a eltűnt a hiba esetén.</span><span class="sxs-lookup"><span data-stu-id="61933-214">Wait for a couple of minutes, the connectivity will be automatically recovered when the error is gone.</span></span>
* <span data-ttu-id="61933-215">Ha a probléma továbbra is fennáll, indítsa újra az átjáró szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="61933-215">If the error persists, restart the gateway service.</span></span>

## <a name="failed-to-author-linked-service"></a><span data-ttu-id="61933-216">Nem sikerült a társított szolgáltatás létrehozásához</span><span class="sxs-lookup"><span data-stu-id="61933-216">Failed to author linked service</span></span>
### <a name="problem"></a><span data-ttu-id="61933-217">Probléma</span><span class="sxs-lookup"><span data-stu-id="61933-217">Problem</span></span>
<span data-ttu-id="61933-218">Ez a hiba jelenhet meg a portálon hitelesítőadat-kezelő segítségével adjon meg egy új társított szolgáltatás hitelesítő adatait, vagy egy meglévő kapcsolt szolgáltatás hitelesítő adatait megkísérlésekor.</span><span class="sxs-lookup"><span data-stu-id="61933-218">You might see this error when you try to use Credential Manager in the portal to input credentials for a new linked service, or update credentials for an existing linked service.</span></span>

`Error: The data store '<Server>/<Database>' cannot be reached. Check connection settings for the data source.`

<span data-ttu-id="61933-219">Amikor megjelenik ez a hiba, a beállítások lapon az adatkezelési átjáró konfigurációkezelőjének nézhet ki például az alábbi képernyőfelvételen.</span><span class="sxs-lookup"><span data-stu-id="61933-219">When you see this error, the settings page of Data Management Gateway Configuration Manager might look like the following screenshot.</span></span>

![Adatbázis nem érhető el](media/data-factory-troubleshoot-gateway-issues/database-cannot-be-reached.png)

#### <a name="cause"></a><span data-ttu-id="61933-221">Ok</span><span class="sxs-lookup"><span data-stu-id="61933-221">Cause</span></span>
<span data-ttu-id="61933-222">Az SSL-tanúsítvány előfordulhat, hogy már megszakadt az átjáró számítógépén.</span><span class="sxs-lookup"><span data-stu-id="61933-222">The SSL certificate might have been lost on the gateway machine.</span></span> <span data-ttu-id="61933-223">Az átjáró-számítógép nem tudja betölteni a jelenleg használt tanúsítvány az SSL-titkosítást.</span><span class="sxs-lookup"><span data-stu-id="61933-223">The gateway computer cannot load the certificate currently that is used for SSL encryption.</span></span> <span data-ttu-id="61933-224">Az eseménynaplóban, az alábbihoz hasonló hibaüzenetet is megjelenhet.</span><span class="sxs-lookup"><span data-stu-id="61933-224">You might also see an error message in the event log that is similar to the following message.</span></span>

 `Unable to get the gateway settings from cloud service. Check the gateway key and the network connection. (Certificate with thumbprint cannot be loaded.)`

#### <a name="resolution"></a><span data-ttu-id="61933-225">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="61933-225">Resolution</span></span>
<span data-ttu-id="61933-226">Kövesse az alábbi lépéseket a probléma megoldásához:</span><span class="sxs-lookup"><span data-stu-id="61933-226">Follow these steps to solve the problem:</span></span>

1. <span data-ttu-id="61933-227">Indítsa el az adatkezelési átjáró Konfigurációkezelőjében.</span><span class="sxs-lookup"><span data-stu-id="61933-227">Start Data Management Gateway Configuration Manager.</span></span>
2. <span data-ttu-id="61933-228">Váltás a **beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="61933-228">Switch to the **Settings** tab.</span></span>  
3. <span data-ttu-id="61933-229">Kattintson a **módosítása** gombra kattintva módosíthatja az SSL-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="61933-229">Click the **Change** button to change the SSL certificate.</span></span>

   ![Módosítás tanúsítvány gombra](media/data-factory-troubleshoot-gateway-issues/change-button-ssl-certificate.png)
4. <span data-ttu-id="61933-231">Jelöljön ki új tanúsítványt, az SSL-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="61933-231">Select a new certificate as the SSL certificate.</span></span> <span data-ttu-id="61933-232">Bármely Ön által létrehozott SSL-tanúsítvány vagy bármely szervezeti is használhatja.</span><span class="sxs-lookup"><span data-stu-id="61933-232">You can use any SSL certificate that is generated by you or any organization.</span></span>

   ![Adja meg a tanúsítvány](media/data-factory-troubleshoot-gateway-issues/specify-http-end-point.png)

## <a name="copy-activity-fails"></a><span data-ttu-id="61933-234">Másolási tevékenység sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="61933-234">Copy activity fails</span></span>
### <a name="problem"></a><span data-ttu-id="61933-235">Probléma</span><span class="sxs-lookup"><span data-stu-id="61933-235">Problem</span></span>
<span data-ttu-id="61933-236">Bizonyára észrevette, hogy a következő "UserErrorFailedToConnectToSqlserver" hiba a portál folyamat beállítása után.</span><span class="sxs-lookup"><span data-stu-id="61933-236">You might notice the following "UserErrorFailedToConnectToSqlserver" failure after you set up a pipeline in the portal.</span></span>

`Error: Copy activity encountered a user error: ErrorCode=UserErrorFailedToConnectToSqlServer,'Type=Microsoft.DataTransfer.Common.Shared.HybridDeliveryException,Message=Cannot connect to SQL Server`

#### <a name="cause"></a><span data-ttu-id="61933-237">Ok</span><span class="sxs-lookup"><span data-stu-id="61933-237">Cause</span></span>
<span data-ttu-id="61933-238">Ez akkor fordulhat elő különböző okokból, és a megoldás ennek megfelelően változik.</span><span class="sxs-lookup"><span data-stu-id="61933-238">This can happen for different reasons, and mitigation varies accordingly.</span></span>

#### <a name="resolution"></a><span data-ttu-id="61933-239">Megoldás:</span><span class="sxs-lookup"><span data-stu-id="61933-239">Resolution</span></span>
<span data-ttu-id="61933-240">Engedélyezi az kimenő TCP-kapcsolatok az adatkezelési átjáró ügyféloldali TCP/1433-as porton keresztül egy SQL-adatbázishoz szeretne csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="61933-240">Allow outbound TCP connections over port TCP/1433 on the Data Management Gateway client side before connecting to an SQL database.</span></span>

<span data-ttu-id="61933-241">Ha a céladatbázis Azure SQL-adatbázis, ellenőrizze az SQL-kiszolgáló a tűzfal beállításai Azure is.</span><span class="sxs-lookup"><span data-stu-id="61933-241">If the target database is an Azure SQL database, check SQL Server firewall settings for Azure as well.</span></span>

<span data-ttu-id="61933-242">A következő részben a helyszíni adattárolóihoz létrehozott kapcsolat ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="61933-242">See the following section to test the connection to the on-premises data store.</span></span>

## <a name="data-store-connection-or-driver-related-errors"></a><span data-ttu-id="61933-243">Az adattároló csatlakozási vagy kapcsolatos hibák</span><span class="sxs-lookup"><span data-stu-id="61933-243">Data store connection or driver-related errors</span></span>
<span data-ttu-id="61933-244">Ha adatok tárolásához a kapcsolódáshoz vagy az illesztőprogram-kapcsolatos hibákat, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="61933-244">If you see data store connection or driver-related errors, complete the following steps:</span></span>

1. <span data-ttu-id="61933-245">Indítsa el az adatkezelési átjáró Konfigurációkezelőjét az átjárót működtető gépen.</span><span class="sxs-lookup"><span data-stu-id="61933-245">Start Data Management Gateway Configuration Manager on the gateway machine.</span></span>
2. <span data-ttu-id="61933-246">Váltás a **diagnosztika** fülre.</span><span class="sxs-lookup"><span data-stu-id="61933-246">Switch to the **Diagnostics** tab.</span></span>
3. <span data-ttu-id="61933-247">A **kapcsolat tesztelése**, az átjáró csoport értékek hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="61933-247">In **Test Connection**, add the gateway group values.</span></span>
4. <span data-ttu-id="61933-248">Kattintson a **teszt** megjelenítéséhez, ha csatlakozhat a helyszíni adatforráshoz az átjárót működtető gépen a kapcsolati adatokat, és a hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="61933-248">Click **Test** to see if you can connect to the on-premises data source from the gateway machine by using the connection information and credentials.</span></span> <span data-ttu-id="61933-249">Amennyiben a kapcsolat tesztelése az illesztő telepítése után is sikertelen, indítsa újra az átjárót, hogy az érvényesítse a legutóbbi módosítást.</span><span class="sxs-lookup"><span data-stu-id="61933-249">If the test connection still fails after you install a driver, restart the gateway for it to pick up the latest change.</span></span>

![Kapcsolat tesztelése a Diagnosztika lap](media/data-factory-troubleshoot-gateway-issues/test-connection-in-diagnostics-tab.png)

## <a name="gateway-logs"></a><span data-ttu-id="61933-251">Átjáró naplói</span><span class="sxs-lookup"><span data-stu-id="61933-251">Gateway logs</span></span>
### <a name="send-gateway-logs-to-microsoft"></a><span data-ttu-id="61933-252">Átjáró naplókat küldeni a Microsoftnak</span><span class="sxs-lookup"><span data-stu-id="61933-252">Send gateway logs to Microsoft</span></span>
<span data-ttu-id="61933-253">Amikor kapcsolatba lép a Microsoft Support átjáró problémák hibaelhárításával kapcsolatos, a program kérheti megosztani az átjáró naplói.</span><span class="sxs-lookup"><span data-stu-id="61933-253">When you contact Microsoft Support to get help with troubleshooting gateway issues, you might be asked to share your gateway logs.</span></span> <span data-ttu-id="61933-254">Az átjáró számára készült kötelező átjáró naplók és a két gombra történő kattintás az adatkezelési átjáró konfigurációkezelőjének is megoszthatja.</span><span class="sxs-lookup"><span data-stu-id="61933-254">With the release of the gateway, you can share required gateway logs with two button clicks in Data Management Gateway Configuration Manager.</span></span>    

1. <span data-ttu-id="61933-255">Váltás a **diagnosztika** az adatkezelési átjáró konfigurációkezelőjének fülre.</span><span class="sxs-lookup"><span data-stu-id="61933-255">Switch to the **Diagnostics** tab in Data Management Gateway Configuration Manager.</span></span>

    ![Felügyeleti átjáró Diagnostics lapon](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-diagnostics-tab.png)
2. <span data-ttu-id="61933-257">Kattintson a **naplók küldése** a következő párbeszédpanel megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="61933-257">Click **Send Logs** to see the following dialog box.</span></span>

    ![Felügyeleti átjáró küldése adatnaplók](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-dialog.png)
3. <span data-ttu-id="61933-259">(Választható) Kattintson a **naplók megtekintése** áttekintéséhez naplózza az esemény viewer.</span><span class="sxs-lookup"><span data-stu-id="61933-259">(Optional) Click **view logs** to review logs in the event viewer.</span></span>
4. <span data-ttu-id="61933-260">(Választható) Kattintson a **adatvédelmi** áttekintése a Microsoft web services adatvédelmi nyilatkozatát.</span><span class="sxs-lookup"><span data-stu-id="61933-260">(Optional) Click **privacy** to review Microsoft web services privacy statement.</span></span>
5. <span data-ttu-id="61933-261">Ha elégedett a feltöltés, kattintson a kívánt esetén **naplók küldése** ténylegesen a naplók az elmúlt hét napban a Microsoftnak küldendő hibaelhárításhoz.</span><span class="sxs-lookup"><span data-stu-id="61933-261">When you are satisfied with what you are about to upload, click **Send Logs** to actually send the logs from the last seven days to Microsoft for troubleshooting.</span></span> <span data-ttu-id="61933-262">Az alábbi képernyőfelvételen látható módon kell megjelennie a küldési-naplók művelet állapotát.</span><span class="sxs-lookup"><span data-stu-id="61933-262">You should see the status of the send-logs operation as shown in the following screenshot.</span></span>

    ![Adatok felügyeleti átjáró küldése naplózza állapota](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-status.png)
6. <span data-ttu-id="61933-264">A művelet befejezése után megjelenik egy párbeszédpanel az alábbi képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="61933-264">After the operation is complete, you see a dialog box as shown in the following screenshot.</span></span>

    ![Adatok felügyeleti átjáró küldése naplózza állapota](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-result.png)
7. <span data-ttu-id="61933-266">Mentse a **azonosítója** és ossza meg a Microsoft Support.</span><span class="sxs-lookup"><span data-stu-id="61933-266">Save the **Report ID** and share it with Microsoft Support.</span></span> <span data-ttu-id="61933-267">A jelentés Azonosítót az átjáró naplók hibaelhárítási feltöltött kereséséhez használható.</span><span class="sxs-lookup"><span data-stu-id="61933-267">The report ID is used to locate the gateway logs that you uploaded for troubleshooting.</span></span>  <span data-ttu-id="61933-268">A jelentés Azonosítót is a menti viewer.</span><span class="sxs-lookup"><span data-stu-id="61933-268">The report ID is also saved in the event viewer.</span></span>  <span data-ttu-id="61933-269">Az eseményazonosító "25" megtekintésével megtalálja, és ellenőrizze a dátum és idő.</span><span class="sxs-lookup"><span data-stu-id="61933-269">You can find it by looking at the event ID “25”, and check the date and time.</span></span>

    ![Adatok felügyeleti átjáró küldése naplózza az azonosítója](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-send-logs-report-id.png)    

### <a name="archive-gateway-logs-on-gateway-host-machine"></a><span data-ttu-id="61933-271">Az átjáró állomás számítógépén archív-átjáró naplói</span><span class="sxs-lookup"><span data-stu-id="61933-271">Archive gateway logs on gateway host machine</span></span>
<span data-ttu-id="61933-272">Léteznek olyan forgatókönyvek, ahol átjáró problémák vannak, és közvetlenül nem osztható meg átjáró naplói:</span><span class="sxs-lookup"><span data-stu-id="61933-272">There are some scenarios where you have gateway issues and you cannot share gateway logs directly:</span></span>

* <span data-ttu-id="61933-273">Manuálisan telepítse az átjárót, és regisztrálja az átjárót.</span><span class="sxs-lookup"><span data-stu-id="61933-273">You manually install the gateway and register the gateway.</span></span>
* <span data-ttu-id="61933-274">Próbálja meg regisztrálni az átjárót az adatkezelési átjáró konfigurációkezelőjének újragenerált kulccsal.</span><span class="sxs-lookup"><span data-stu-id="61933-274">You try to register the gateway with a regenerated key in Data Management Gateway Configuration Manager.</span></span>
* <span data-ttu-id="61933-275">Próbálja meg elküldeni a naplókat, és nem lehet csatlakozni az átjáró gazdaszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="61933-275">You try to send logs and the gateway host service cannot be connected.</span></span>

<span data-ttu-id="61933-276">Ezek a forgatókönyvek az átjáró naplói elmentse egy zip-fájlt, és ossza meg, amikor a Microsoft támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="61933-276">For these scenarios, you can save gateway logs as a zip file and share it when you contact Microsoft support.</span></span> <span data-ttu-id="61933-277">Például ha arra vonatkozó hibaüzenetet kap, amíg az átjáró, regisztrálnia jelenik meg az alábbi képernyőfelvételen.</span><span class="sxs-lookup"><span data-stu-id="61933-277">For example, if you receive an error while you register the gateway as shown in the following screenshot.</span></span>   

![Felügyeleti átjáró regisztrációs hiba](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-registration-error.png)

<span data-ttu-id="61933-279">Kattintson a **átjáró naplók archiválása** csatolása archivált, és mentse a naplókat, és majd a zip-fájl megosztása a Microsoft támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="61933-279">Click the **Archive gateway logs** link to archive and save logs, and then share the zip file with Microsoft support.</span></span>

![Felügyeleti átjáró archív adatnaplók](media/data-factory-troubleshoot-gateway-issues/data-management-gateway-archive-logs.png)

### <a name="locate-gateway-logs"></a><span data-ttu-id="61933-281">Keresse meg az átjáró naplói</span><span class="sxs-lookup"><span data-stu-id="61933-281">Locate gateway logs</span></span>
<span data-ttu-id="61933-282">A Windows eseménynaplóiban keresse meg a részletes átjáró naplóadatok találja.</span><span class="sxs-lookup"><span data-stu-id="61933-282">You can find detailed gateway log information in the Windows event logs.</span></span>

1. <span data-ttu-id="61933-283">A Windows indítása **Eseménynapló**.</span><span class="sxs-lookup"><span data-stu-id="61933-283">Start Windows **Event Viewer**.</span></span>
2. <span data-ttu-id="61933-284">Keresse meg a naplókat a **alkalmazás- és szolgáltatásnaplók** > **az adatkezelési átjáró** mappát.</span><span class="sxs-lookup"><span data-stu-id="61933-284">Locate logs in the **Application and Services Logs** > **Data Management Gateway** folder.</span></span>

 <span data-ttu-id="61933-285">Átjáró kapcsolatos problémák elhárításakor még keressen szintű hibaesemények az Eseménynapló viewer.</span><span class="sxs-lookup"><span data-stu-id="61933-285">When you're troubleshooting gateway-related issues, look for error level events in the event viewer.</span></span>

![Az adatkezelési átjáró naplózza az eseménynaplóban](media/data-factory-troubleshoot-gateway-issues/gateway-logs-event-viewer.png)
