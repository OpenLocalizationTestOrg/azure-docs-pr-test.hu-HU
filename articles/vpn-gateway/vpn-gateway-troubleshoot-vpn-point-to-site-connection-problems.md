---
title: "Azure pont-pont aaaTroubleshoot kapcsolódási problémák |} Microsoft Docs"
description: "Megtudhatja, hogyan tootroubleshoot pont-hely kapcsolódási problémák léptek fel."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: genli
ms.openlocfilehash: 98d66074be62ad8c7153a903f69cb0d01f988cd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-point-to-site-connection-problems"></a><span data-ttu-id="4934e-103">Hibáinak elhárítása: Az Azure pont – hely kapcsolat problémák</span><span class="sxs-lookup"><span data-stu-id="4934e-103">Troubleshooting: Azure point-to-site connection problems</span></span>

<span data-ttu-id="4934e-104">A cikk ismerteti, amelyekkel Ön is szembesülhet pont-pont csatlakozási probléma.</span><span class="sxs-lookup"><span data-stu-id="4934e-104">This article lists common point-to-site connection problems that you might experience.</span></span> <span data-ttu-id="4934e-105">Lehetséges okait és megoldásait ezeket a problémákat is ismerteti.</span><span class="sxs-lookup"><span data-stu-id="4934e-105">It also discusses possible causes and solutions for these problems.</span></span>

## <a name="vpn-client-error-a-certificate-could-not-be-found"></a><span data-ttu-id="4934e-106">VPN-ügyfél hiba: A tanúsítvány nem található.</span><span class="sxs-lookup"><span data-stu-id="4934e-106">VPN client error: A certificate could not be found</span></span>

### <a name="symptom"></a><span data-ttu-id="4934e-107">Jelenség</span><span class="sxs-lookup"><span data-stu-id="4934e-107">Symptom</span></span>

<span data-ttu-id="4934e-108">Azure-beli virtuális hálózat tooconnect tooan hello VPN-ügyfél segítségével kísérli meg, a hello a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="4934e-108">When you try tooconnect tooan Azure virtual network by using hello VPN client, you receive hello following error message:</span></span>

<span data-ttu-id="4934e-109">**A tanúsítvány nem található, amely használható a bővíthető hitelesítési protokoll. (798 hiba)**</span><span class="sxs-lookup"><span data-stu-id="4934e-109">**A certificate could not be found that can be used with this Extensible Authentication Protocol. (Error 798)**</span></span>

### <a name="cause"></a><span data-ttu-id="4934e-110">Ok</span><span class="sxs-lookup"><span data-stu-id="4934e-110">Cause</span></span>

<span data-ttu-id="4934e-111">Ez a probléma akkor fordul elő, ha hello ügyféltanúsítvány hiányzik a **tanúsítványok - aktuális User\Personal\Certificates**.</span><span class="sxs-lookup"><span data-stu-id="4934e-111">This problem occurs if hello client certificate is missing from **Certificates - Current User\Personal\Certificates**.</span></span>

### <a name="solution"></a><span data-ttu-id="4934e-112">Megoldás</span><span class="sxs-lookup"><span data-stu-id="4934e-112">Solution</span></span>

<span data-ttu-id="4934e-113">Győződjön meg arról, hogy hello ügyfél tanúsítvány telepítve van a hello hello tanúsítványtárolóba (Certmgr.msc) helye a következő:</span><span class="sxs-lookup"><span data-stu-id="4934e-113">Make sure that hello client certificate is installed in hello following location of hello Certificates store (Certmgr.msc):</span></span>
 
<span data-ttu-id="4934e-114">**Tanúsítványok – aktuális User\Personal\Certificates**</span><span class="sxs-lookup"><span data-stu-id="4934e-114">**Certificates - Current User\Personal\Certificates**</span></span>

<span data-ttu-id="4934e-115">Hogyan tooinstall hello ügyféltanúsítvány kapcsolatos további információkért lásd: [tanúsítvány létrehozása és exportálása a pont – hely kapcsolatok](vpn-gateway-certificates-point-to-site.md).</span><span class="sxs-lookup"><span data-stu-id="4934e-115">For more information about how tooinstall hello client certificate, see [Generate and export certificates for point-to-site connections](vpn-gateway-certificates-point-to-site.md).</span></span>

> [!NOTE]
> <span data-ttu-id="4934e-116">Hello ügyféltanúsítvány importálásakor, ne válassza hello **titkos kulcs erős védelmének engedélyezése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4934e-116">When you import hello client certificate, do not select hello **Enable strong private key protection** option.</span></span>

## <a name="vpn-client-error-hello-message-received-was-unexpected-or-badly-formatted"></a><span data-ttu-id="4934e-117">VPN-ügyfél hiba: hello fogadott üzenet nem várt vagy rosszul formázott</span><span class="sxs-lookup"><span data-stu-id="4934e-117">VPN client error: hello message received was unexpected or badly formatted</span></span>

### <a name="symptom"></a><span data-ttu-id="4934e-118">Jelenség</span><span class="sxs-lookup"><span data-stu-id="4934e-118">Symptom</span></span>

<span data-ttu-id="4934e-119">Azure-beli virtuális hálózat tooconnect tooan hello VPN-ügyfél segítségével kísérli meg, a hello a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="4934e-119">When you try tooconnect tooan Azure virtual network by using hello VPN client, you receive hello following error message:</span></span>

<span data-ttu-id="4934e-120">**hello üzenet érkezett a váratlan vagy rosszul formázott volt. (0x80090326 hiba)**</span><span class="sxs-lookup"><span data-stu-id="4934e-120">**hello message received was unexpected or badly formatted. (Error 0x80090326)**</span></span>

### <a name="cause"></a><span data-ttu-id="4934e-121">Ok</span><span class="sxs-lookup"><span data-stu-id="4934e-121">Cause</span></span>

<span data-ttu-id="4934e-122">Ez a probléma akkor fordul elő, ha hello legfelső szintű tanúsítvány nyilvános kulcsa nem hello Azure VPN gateway van töltve.</span><span class="sxs-lookup"><span data-stu-id="4934e-122">This problem occurs if hello root certificate public key is not uploaded into hello Azure VPN gateway.</span></span> <span data-ttu-id="4934e-123">Ez akkor is előfordulhat, ha hello kulcs sérült vagy lejárt.</span><span class="sxs-lookup"><span data-stu-id="4934e-123">It can also occur if hello key is corrupted or expired.</span></span>

### <a name="solution"></a><span data-ttu-id="4934e-124">Megoldás</span><span class="sxs-lookup"><span data-stu-id="4934e-124">Solution</span></span>

<span data-ttu-id="4934e-125">tooresolve probléma hello állapotát hello legfelső szintű tanúsítványt hello Azure portál toosee e visszavonásra került.</span><span class="sxs-lookup"><span data-stu-id="4934e-125">tooresolve this problem, check hello status of hello root certificate in hello Azure portal toosee whether it was revoked.</span></span> <span data-ttu-id="4934e-126">Ha nincs visszavonva, próbálja meg toodelete hello legfelső szintű tanúsítvány és reupload.</span><span class="sxs-lookup"><span data-stu-id="4934e-126">If it is not revoked, try toodelete hello root certificate and reupload.</span></span> <span data-ttu-id="4934e-127">További információkért lásd: [olyan tanúsítványokat hoznak létre](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).</span><span class="sxs-lookup"><span data-stu-id="4934e-127">For more information, see [Create certificates](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).</span></span>

## <a name="vpn-client-error-a-certificate-chain-processed-but-terminated"></a><span data-ttu-id="4934e-128">VPN-ügyfél hiba: A tanúsítványlánc feldolgozása, de a megszakadt</span><span class="sxs-lookup"><span data-stu-id="4934e-128">VPN client error: A certificate chain processed but terminated</span></span> 

### <a name="symptom"></a><span data-ttu-id="4934e-129">Jelenség</span><span class="sxs-lookup"><span data-stu-id="4934e-129">Symptom</span></span> 

<span data-ttu-id="4934e-130">Azure-beli virtuális hálózat tooconnect tooan hello VPN-ügyfél segítségével kísérli meg, a hello a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="4934e-130">When you try tooconnect tooan Azure virtual network by using hello VPN client, you receive hello following error message:</span></span>

<span data-ttu-id="4934e-131">**Tanúsítványlánc feldolgozása befejeződött, de a hello megbízhatóság-ellenőrző nem megbízható legfelső szintű tanúsítványt megszakadt.**</span><span class="sxs-lookup"><span data-stu-id="4934e-131">**A certificate chain processed but terminated in a root certificate which is not trusted by hello trust provider.**</span></span>

### <a name="solution"></a><span data-ttu-id="4934e-132">Megoldás</span><span class="sxs-lookup"><span data-stu-id="4934e-132">Solution</span></span>

1. <span data-ttu-id="4934e-133">Győződjön meg arról, hogy a következő tanúsítványok hello hello megfelelő helyen találhatók:</span><span class="sxs-lookup"><span data-stu-id="4934e-133">Make sure that hello following certificates are in hello correct location:</span></span>

    | <span data-ttu-id="4934e-134">Tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="4934e-134">Certificate</span></span> | <span data-ttu-id="4934e-135">Hely</span><span class="sxs-lookup"><span data-stu-id="4934e-135">Location</span></span> |
    | ------------- | ------------- |
    | <span data-ttu-id="4934e-136">AzureClient.pfx</span><span class="sxs-lookup"><span data-stu-id="4934e-136">AzureClient.pfx</span></span>  | <span data-ttu-id="4934e-137">Aktuális User\Personal\Certificates</span><span class="sxs-lookup"><span data-stu-id="4934e-137">Current User\Personal\Certificates</span></span> |
    | <span data-ttu-id="4934e-138">Azuregateway -*GUID*. cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="4934e-138">Azuregateway-*GUID*.cloudapp.net</span></span>  | <span data-ttu-id="4934e-139">Aktuális User\Trusted legfelső szintű hitelesítésszolgáltatók</span><span class="sxs-lookup"><span data-stu-id="4934e-139">Current User\Trusted Root Certification Authorities</span></span>|
    | <span data-ttu-id="4934e-140">AzureGateway -*GUID*. cloudapp.net, AzureRoot.cer</span><span class="sxs-lookup"><span data-stu-id="4934e-140">AzureGateway-*GUID*.cloudapp.net, AzureRoot.cer</span></span>    | <span data-ttu-id="4934e-141">Helyi számítógép\Megbízható legfelső szintű hitelesítésszolgáltatók</span><span class="sxs-lookup"><span data-stu-id="4934e-141">Local Computer\Trusted Root Certification Authorities</span></span>|

2. <span data-ttu-id="4934e-142">Ha hello tanúsítványok már hello helyen, próbálja toodelete hello tanúsítványokat, és telepítse újra azokat.</span><span class="sxs-lookup"><span data-stu-id="4934e-142">If hello certificates are already in hello location, try toodelete hello certificates and reinstall them.</span></span> <span data-ttu-id="4934e-143">Hello  **azuregateway -*GUID*. cloudapp.net** tanúsítvány van hello VPN-konfigurációs ügyfélcsomag beszerzett hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="4934e-143">hello **azuregateway-*GUID*.cloudapp.net** certificate is in hello VPN client configuration package that you downloaded from hello Azure portal.</span></span> <span data-ttu-id="4934e-144">Használhatja a fájl archivers tooextract hello fájlokat hello csomagból.</span><span class="sxs-lookup"><span data-stu-id="4934e-144">You can use file archivers tooextract hello files from hello package.</span></span>

## <a name="file-download-error-target-uri-is-not-specified"></a><span data-ttu-id="4934e-145">Letöltési hiba: nincs megadva a cél URI Azonosítóját</span><span class="sxs-lookup"><span data-stu-id="4934e-145">File download error: Target URI is not specified</span></span>

### <a name="symptom"></a><span data-ttu-id="4934e-146">Jelenség</span><span class="sxs-lookup"><span data-stu-id="4934e-146">Symptom</span></span>

<span data-ttu-id="4934e-147">Hello a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="4934e-147">You receive hello following error message:</span></span>

<span data-ttu-id="4934e-148">**Hiba történt a fájl letöltése. Nincs megadva a cél URI Azonosítóját.**</span><span class="sxs-lookup"><span data-stu-id="4934e-148">**File download error. Target URI is not specified.**</span></span>

### <a name="cause"></a><span data-ttu-id="4934e-149">Ok</span><span class="sxs-lookup"><span data-stu-id="4934e-149">Cause</span></span> 

<span data-ttu-id="4934e-150">A probléma miatt egy hibás átjáró típusa.</span><span class="sxs-lookup"><span data-stu-id="4934e-150">This problem occurs because of an incorrect gateway type.</span></span> 

### <a name="solution"></a><span data-ttu-id="4934e-151">Megoldás</span><span class="sxs-lookup"><span data-stu-id="4934e-151">Solution</span></span>

<span data-ttu-id="4934e-152">VPN-átjáró típusa hello kell **VPN**, és a VPN-típus hello kell **RouteBased**.</span><span class="sxs-lookup"><span data-stu-id="4934e-152">hello VPN gateway type must be **VPN**, and hello VPN type must be **RouteBased**.</span></span>

## <a name="vpn-client-error-azure-vpn-custom-script-failed"></a><span data-ttu-id="4934e-153">VPN-ügyfél hiba: az Azure VPN egyéni parancsprogram végrehajtása sikertelen volt</span><span class="sxs-lookup"><span data-stu-id="4934e-153">VPN client error: Azure VPN custom script failed</span></span> 

### <a name="symptom"></a><span data-ttu-id="4934e-154">Jelenség</span><span class="sxs-lookup"><span data-stu-id="4934e-154">Symptom</span></span>

<span data-ttu-id="4934e-155">Azure-beli virtuális hálózat tooconnect tooan hello VPN-ügyfél segítségével kísérli meg, a hello a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="4934e-155">When you try tooconnect tooan Azure virtual network by using hello VPN client, you receive hello following error message:</span></span>

<span data-ttu-id="4934e-156">**Egyéni parancsfájl (tooupdate az útválasztási táblázatot) nem sikerült. (8007026f hiba)**</span><span class="sxs-lookup"><span data-stu-id="4934e-156">**Custom script (tooupdate your routing table) failed. (Error 8007026f)**</span></span>

### <a name="cause"></a><span data-ttu-id="4934e-157">Ok</span><span class="sxs-lookup"><span data-stu-id="4934e-157">Cause</span></span>

<span data-ttu-id="4934e-158">Ez a probléma akkor fordulhat elő, ha tooopen hello pont hely közötti VPN-kapcsolatot egy helyi használatával próbálja.</span><span class="sxs-lookup"><span data-stu-id="4934e-158">This problem might occur if you are trying tooopen hello site-to-point VPN connection by using a shortcut.</span></span>

### <a name="solution"></a><span data-ttu-id="4934e-159">Megoldás</span><span class="sxs-lookup"><span data-stu-id="4934e-159">Solution</span></span> 

<span data-ttu-id="4934e-160">Nyissa meg a hello VPN csomag közvetlenül nem kell megnyitnia az hello helyi.</span><span class="sxs-lookup"><span data-stu-id="4934e-160">Open hello VPN package directly instead of opening it from hello shortcut.</span></span>

## <a name="cannot-install-hello-vpn-client"></a><span data-ttu-id="4934e-161">Hello VPN-ügyfél nem tudja telepíteni.</span><span class="sxs-lookup"><span data-stu-id="4934e-161">Cannot install hello VPN client</span></span>

### <a name="cause"></a><span data-ttu-id="4934e-162">Ok</span><span class="sxs-lookup"><span data-stu-id="4934e-162">Cause</span></span> 

<span data-ttu-id="4934e-163">Kiegészítő igazolás szükség tootrust hello VPN-átjáró a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="4934e-163">An additional certificate is required tootrust hello VPN gateway for your virtual network.</span></span> <span data-ttu-id="4934e-164">hello tanúsítvány hello VPN-konfigurációs ügyfélcsomag a hello Azure-portálon létrehozott tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="4934e-164">hello certificate is included in hello VPN client configuration package that is generated from hello Azure portal.</span></span>

### <a name="solution"></a><span data-ttu-id="4934e-165">Megoldás</span><span class="sxs-lookup"><span data-stu-id="4934e-165">Solution</span></span>

<span data-ttu-id="4934e-166">Bontsa ki a hello VPN-ügyfélcsomag konfigurációs, és hello .cer fájl található.</span><span class="sxs-lookup"><span data-stu-id="4934e-166">Extract hello VPN client configuration package, and find hello .cer file.</span></span> <span data-ttu-id="4934e-167">tooinstall hello tanúsítvány, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4934e-167">tooinstall hello certificate, follow these steps:</span></span>

1. <span data-ttu-id="4934e-168">Nyissa meg a mmc.exe.</span><span class="sxs-lookup"><span data-stu-id="4934e-168">Open mmc.exe.</span></span>
2. <span data-ttu-id="4934e-169">Adja hozzá a hello **tanúsítványok** beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="4934e-169">Add hello **Certificates** snap-in.</span></span>
3. <span data-ttu-id="4934e-170">Jelölje be hello **számítógép** fiókot a helyi számítógép hello.</span><span class="sxs-lookup"><span data-stu-id="4934e-170">Select hello **Computer** account for hello local computer.</span></span>
4. <span data-ttu-id="4934e-171">Kattintson a jobb gombbal hello **megbízható legfelső szintű hitelesítésszolgáltatók** csomópont.</span><span class="sxs-lookup"><span data-stu-id="4934e-171">Right-click hello **Trusted Root Certification Authorities** node.</span></span> <span data-ttu-id="4934e-172">Kattintson a **minden-tevékenység** > **importálása**, és tallózással keresse meg toohello .cer fájl hello VPN-ügyfélcsomag konfigurációs kibontott.</span><span class="sxs-lookup"><span data-stu-id="4934e-172">Click **All-Task** > **Import**, and browse toohello .cer file you extracted from hello VPN client configuration package.</span></span>
5. <span data-ttu-id="4934e-173">Indítsa újra a hello számítógépet.</span><span class="sxs-lookup"><span data-stu-id="4934e-173">Restart hello computer.</span></span> 
6. <span data-ttu-id="4934e-174">Próbálja meg tooinstall hello VPN-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="4934e-174">Try tooinstall hello VPN client.</span></span>

## <a name="azure-portal-error-failed-toosave-hello-vpn-gateway-and-hello-data-is-invalid"></a><span data-ttu-id="4934e-175">Az Azure portál hiba: nem sikerült toosave hello VPN-átjárót, és hello adat érvénytelen</span><span class="sxs-lookup"><span data-stu-id="4934e-175">Azure portal error: Failed toosave hello VPN gateway, and hello data is invalid</span></span>

### <a name="symptom"></a><span data-ttu-id="4934e-176">Jelenség</span><span class="sxs-lookup"><span data-stu-id="4934e-176">Symptom</span></span>

<span data-ttu-id="4934e-177">Amikor toosave hello változásokat a VPN-átjáró hello hello Azure-portálon, hello a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="4934e-177">When you try toosave hello changes for hello VPN gateway in hello Azure portal, you receive hello following error message:</span></span>

<span data-ttu-id="4934e-178">**Virtuális hálózati átjáró hibás toosave &lt;* átjárónevet*&gt;.</span><span class="sxs-lookup"><span data-stu-id="4934e-178">**Failed toosave virtual network gateway &lt;*gateway name*&gt;.</span></span> <span data-ttu-id="4934e-179">Tanúsítvány adatainak &lt; *tanúsítvány azonosító* &gt; van invalid.* *</span><span class="sxs-lookup"><span data-stu-id="4934e-179">Data for certificate &lt;*certificate ID*&gt; is invalid.**</span></span>

### <a name="cause"></a><span data-ttu-id="4934e-180">Ok</span><span class="sxs-lookup"><span data-stu-id="4934e-180">Cause</span></span> 

<span data-ttu-id="4934e-181">Ez a probléma akkor fordulhat elő, ha hello legfelső szintű tanúsítvány nyilvános kulcsa feltöltött tartalmaz egy érvénytelen karakter, például egy szóközzel.</span><span class="sxs-lookup"><span data-stu-id="4934e-181">This problem might occur if hello root certificate public key that you uploaded contains an invalid character, such as a space.</span></span>

### <a name="solution"></a><span data-ttu-id="4934e-182">Megoldás</span><span class="sxs-lookup"><span data-stu-id="4934e-182">Solution</span></span>

<span data-ttu-id="4934e-183">Győződjön meg arról, hogy hello adatok hello tanúsítvány nem tartalmaz érvénytelen karaktereket, például sortörést (kocsivissza).</span><span class="sxs-lookup"><span data-stu-id="4934e-183">Make sure that hello data in hello certificate does not contain invalid characters, such as line breaks (carriage returns).</span></span> <span data-ttu-id="4934e-184">hello egész értéknek kell lennie egy hosszú sor.</span><span class="sxs-lookup"><span data-stu-id="4934e-184">hello entire value should be one long line.</span></span> <span data-ttu-id="4934e-185">a következő szöveg hello látható egy minta hello tanúsítvány:</span><span class="sxs-lookup"><span data-stu-id="4934e-185">hello following text is a sample of hello certificate:</span></span>

    -----BEGIN CERTIFICATE-----
    MIIC5zCCAc+gAwIBAgIQFSwsLuUrCIdHwI3hzJbdBjANBgkqhkiG9w0BAQsFADAW
    MRQwEgYDVQQDDAtQMlNSb290Q2VydDAeFw0xNzA2MTUwMjU4NDZaFw0xODA2MTUw
    MzE4NDZaMBYxFDASBgNVBAMMC1AyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEF
    AAOCAQ8AMIIBCgKCAQEAz8QUCWCxxxTrxF5yc5uUpL/bzwC5zZ804ltB1NpPa/PI
    sa5uwLw/YFb8XG/JCWxUJpUzS/kHUKFluqkY80U+fAmRmTEMq5wcaMhp3wRfeq+1
    G9OPBNTyqpnHe+i54QAnj1DjsHXXNL4AL1N8/TSzYTm7dkiq+EAIyRRMrZlYwije
    407ChxIp0stB84MtMShhyoSm2hgl+3zfwuaGXoJQwWiXh715kMHVTSj9zFechYd7
    5OLltoRRDyyxsf0qweTFKIgFj13Hn/bq/UJG3AcyQNvlCv1HwQnXO+hckVBB29wE
    sF8QSYk2MMGimPDYYt4ZM5tmYLxxxvGmrGhc+HWXzMeQIDAQABozEwLzAOBgNVHQ8B
    Af8EBAMCAgQwHQYDVR0OBBYEFBE9zZWhQftVLBQNATC/LHLvMb0OMA0GCSqGSIb3
    DQEBCwUAA4IBAQB7k0ySFUQu72sfj3BdNxrXSyOT4L2rADLhxxxiK0U6gHUF6eWz
    /0h6y4mNkg3NgLT3j/WclqzHXZruhWAXSF+VbAGkwcKA99xGWOcUJ+vKVYL/kDja
    gaZrxHlhTYVVmwn4F7DWhteFqhzZ89/W9Mv6p180AimF96qDU8Ez8t860HQaFkU6
    2Nw9ZMsGkvLePZZi78yVBDCWMogBMhrRVXG/xQkBajgvL5syLwFBo2kWGdC+wyWY
    U/Z+EK9UuHnn3Hkq/vXEzRVsYuaxchta0X2UNRzRq+o706l+iyLTpe6fnvW6ilOi
    e8Jcej7mzunzyjz4chN0/WVF94MtxbUkLkqP
    -----END CERTIFICATE-----

## <a name="azure-portal-error-failed-toosave-hello-vpn-gateway-and-hello-resource-name-is-invalid"></a><span data-ttu-id="4934e-186">Az Azure portál hiba: nem sikerült toosave hello VPN-átjáró, és hello erőforrás neve érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="4934e-186">Azure portal error: Failed toosave hello VPN gateway, and hello resource name is invalid</span></span>

### <a name="symptom"></a><span data-ttu-id="4934e-187">Jelenség</span><span class="sxs-lookup"><span data-stu-id="4934e-187">Symptom</span></span>

<span data-ttu-id="4934e-188">Amikor toosave hello változásokat a VPN-átjáró hello hello Azure-portálon, hello a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="4934e-188">When you try toosave hello changes for hello VPN gateway in hello Azure portal, you receive hello following error message:</span></span> 

<span data-ttu-id="4934e-189">**Virtuális hálózati átjáró hibás toosave &lt;* átjárónevet*&gt;.</span><span class="sxs-lookup"><span data-stu-id="4934e-189">**Failed toosave virtual network gateway &lt;*gateway name*&gt;.</span></span> <span data-ttu-id="4934e-190">Az erőforrásnév &lt; *tooupload meg a tanúsítvány neve* &gt; van érvénytelen **.</span><span class="sxs-lookup"><span data-stu-id="4934e-190">Resource name &lt;*certificate name you try tooupload*&gt; is invalid**.</span></span>

### <a name="cause"></a><span data-ttu-id="4934e-191">Ok</span><span class="sxs-lookup"><span data-stu-id="4934e-191">Cause</span></span>

<span data-ttu-id="4934e-192">A probléma oka, hogy hello tanúsítvány hello neve érvénytelen karaktert, például egy szóközzel tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4934e-192">This problem occurs because hello name of hello certificate contains an invalid character, such as a space.</span></span> 

## <a name="azure-portal-error-vpn-package-file-download-error-503"></a><span data-ttu-id="4934e-193">Az Azure portál hiba: VPN csomag fájl letöltési hiba 503-as</span><span class="sxs-lookup"><span data-stu-id="4934e-193">Azure portal error: VPN package file download error 503</span></span>

### <a name="symptom"></a><span data-ttu-id="4934e-194">Jelenség</span><span class="sxs-lookup"><span data-stu-id="4934e-194">Symptom</span></span>

<span data-ttu-id="4934e-195">Amikor toodownload konfigurációs hello VPN-ügyfélcsomag, hello a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="4934e-195">When you try toodownload hello VPN client configuration package, you receive hello following error message:</span></span>

<span data-ttu-id="4934e-196">**Nem sikerült toodownload hello fájlt. Hiba legutolsó részletes adatai: 503-as hiba. hello kiszolgáló túlterhelt.**</span><span class="sxs-lookup"><span data-stu-id="4934e-196">**Failed toodownload hello file. Error details: error 503. hello server is busy.**</span></span>
 
### <a name="solution"></a><span data-ttu-id="4934e-197">Megoldás</span><span class="sxs-lookup"><span data-stu-id="4934e-197">Solution</span></span>

<span data-ttu-id="4934e-198">Ez a hiba átmeneti hálózati probléma okozhatja.</span><span class="sxs-lookup"><span data-stu-id="4934e-198">This error can be caused by a temporary network problem.</span></span> <span data-ttu-id="4934e-199">Toodownload hello VPN csomag próbálkozzon újra néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="4934e-199">Try toodownload hello VPN package again after a few minutes.</span></span>

## <a name="azure-vpn-gateway-upgrade-all-p2s-clients-are-unable-tooconnect"></a><span data-ttu-id="4934e-200">Az Azure VPN Gateway frissítése: minden P2S-ügyfelek nem tooconnect</span><span class="sxs-lookup"><span data-stu-id="4934e-200">Azure VPN Gateway upgrade: All P2S clients are unable tooconnect</span></span>

### <a name="cause"></a><span data-ttu-id="4934e-201">Ok</span><span class="sxs-lookup"><span data-stu-id="4934e-201">Cause</span></span>

<span data-ttu-id="4934e-202">Ha hello tanúsítvány több mint 50 %-a teljes élettartama keresztül hello tanúsítvány frissítése.</span><span class="sxs-lookup"><span data-stu-id="4934e-202">If hello certificate is more than 50 percent through its lifetime, hello certificate is rolled over.</span></span>

### <a name="solution"></a><span data-ttu-id="4934e-203">Megoldás</span><span class="sxs-lookup"><span data-stu-id="4934e-203">Solution</span></span>

<span data-ttu-id="4934e-204">tooresolve probléma létrehozása, és újra elosztják a VPN-ügyfelek új tanúsítványok toohello.</span><span class="sxs-lookup"><span data-stu-id="4934e-204">tooresolve this problem, create and redistribute new certificates toohello VPN clients.</span></span> 

## <a name="too-many-vpn-clients-connected-at-once"></a><span data-ttu-id="4934e-205">Túl sok a VPN-ügyfelek egyszerre csatlakoztatva</span><span class="sxs-lookup"><span data-stu-id="4934e-205">Too many VPN clients connected at once</span></span>

<span data-ttu-id="4934e-206">Minden egyes VPN-átjáró hello engedélyezett kapcsolatok maximális száma 128 esetén.</span><span class="sxs-lookup"><span data-stu-id="4934e-206">For each VPN gateway, hello maximum number of allowable connections is 128.</span></span> <span data-ttu-id="4934e-207">Láthatja, hogy hello hello Azure-portálon a csatlakoztatott ügyfelek teljes száma.</span><span class="sxs-lookup"><span data-stu-id="4934e-207">You can see hello total number of connected clients in hello Azure portal.</span></span>

## <a name="point-to-site-vpn-incorrectly-adds-a-route-for-100008-toohello-route-table"></a><span data-ttu-id="4934e-208">Pont – hely típusú VPN helytelenül 10.0.0.0/8 toohello útvonaltábla útvonal hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4934e-208">Point-to-site VPN incorrectly adds a route for 10.0.0.0/8 toohello route table</span></span>

### <a name="symptom"></a><span data-ttu-id="4934e-209">Jelenség</span><span class="sxs-lookup"><span data-stu-id="4934e-209">Symptom</span></span>

<span data-ttu-id="4934e-210">Amikor hello hello pont-pont ügyfél VPN-kapcsolatot, a hello VPN-ügyfél kell hello Azure-beli virtuális hálózat felé útvonal hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="4934e-210">When you dial hello VPN connection on hello point-to-site client, hello VPN client should add a route toward hello Azure virtual network.</span></span> <span data-ttu-id="4934e-211">hello IP-segítő szolgáltatás hozzá kell adnia egy útvonal hello alhálózat hello VPN-ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="4934e-211">hello IP helper service should add a route for hello subnet of hello VPN clients.</span></span> 

<span data-ttu-id="4934e-212">VPN-ügyfél tartomány hello tooa kisebb alhálózata 10.0.0.0/8, például a 10.0.12.0/24 tartozik.</span><span class="sxs-lookup"><span data-stu-id="4934e-212">hello VPN client range belongs tooa smaller subnet of 10.0.0.0/8, such as 10.0.12.0/24.</span></span> <span data-ttu-id="4934e-213">Helyett 10.0.12.0/24 útvonal, a 10.0.0.0/8 adjon hozzá egy útvonalat magasabb prioritással bír, amely.</span><span class="sxs-lookup"><span data-stu-id="4934e-213">Instead of a route for 10.0.12.0/24, a route for 10.0.0.0/8 is added that has higher priority.</span></span> 

<span data-ttu-id="4934e-214">Ez helytelen az útvonal más hálózatokkal helyszíni tooanother alhálózati tartományba hello 10.0.0.0/8, például a 10.50.0.0/24, előfordulhat, hogy tartoznak, amelyek nem rendelkeznek egy adott útvonal definiálva kapcsolat megszakad.</span><span class="sxs-lookup"><span data-stu-id="4934e-214">This incorrect route breaks connectivity with other on-premises networks that might belong tooanother subnet within hello 10.0.0.0/8 range, such as 10.50.0.0/24, that don't have a specific route defined.</span></span> 

### <a name="cause"></a><span data-ttu-id="4934e-215">Ok</span><span class="sxs-lookup"><span data-stu-id="4934e-215">Cause</span></span>

<span data-ttu-id="4934e-216">Ez szándékosan van a Windows-ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="4934e-216">This behavior is by design for Windows clients.</span></span> <span data-ttu-id="4934e-217">Hello ügyfél hello PPP IPCP protokollt használja, amikor hello kiszolgálótól (ebben az esetben hello VPN-átjáró) beszerzett hello alagútkapcsolat hello IP-címet.</span><span class="sxs-lookup"><span data-stu-id="4934e-217">When hello client uses hello PPP IPCP protocol, it obtains hello IP address for hello tunnel interface from hello server (hello VPN gateway in this case).</span></span> <span data-ttu-id="4934e-218">Azonban hello protokoll korlátozása, miatt a hello ügyfél nem rendelkezik hello alhálózati maszkot.</span><span class="sxs-lookup"><span data-stu-id="4934e-218">However, because of a limitation in hello protocol, hello client does not have hello subnet mask.</span></span> <span data-ttu-id="4934e-219">Mivel nincs más módja tooget, hello ügyfél megkísérli tooguess hello alhálózati maszk hello osztály hello alagút illesztő IP-cím alapján.</span><span class="sxs-lookup"><span data-stu-id="4934e-219">Because there is no other way tooget it, hello client tries tooguess hello subnet mask based on hello class of hello tunnel interface IP address.</span></span> 

<span data-ttu-id="4934e-220">Ezért adjon hozzá egy útvonalat hello statikus leképezés a következő alapján:</span><span class="sxs-lookup"><span data-stu-id="4934e-220">Therefore, a route is added based on hello following static mapping:</span></span> 

<span data-ttu-id="4934e-221">Ha a cím tartozik tooclass A--> alkalmazása /8</span><span class="sxs-lookup"><span data-stu-id="4934e-221">If address belongs tooclass A --> apply /8</span></span>

<span data-ttu-id="4934e-222">Ha a cím tartozik B--> tooclass alkalmazása /16</span><span class="sxs-lookup"><span data-stu-id="4934e-222">If address belongs tooclass B --> apply /16</span></span>

<span data-ttu-id="4934e-223">Ha a cím tartozik C--> tooclass alkalmazása /24</span><span class="sxs-lookup"><span data-stu-id="4934e-223">If address belongs tooclass C --> apply /24</span></span>

## <a name="vpn-client-cannot-access-network-file-shares"></a><span data-ttu-id="4934e-224">VPN-ügyfél nem tud hozzáférni a hálózati fájlmegosztások</span><span class="sxs-lookup"><span data-stu-id="4934e-224">VPN client cannot access network file shares</span></span>

### <a name="symptom"></a><span data-ttu-id="4934e-225">Jelenség</span><span class="sxs-lookup"><span data-stu-id="4934e-225">Symptom</span></span>

<span data-ttu-id="4934e-226">hello VPN-ügyfél csatlakozott toohello Azure-beli virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="4934e-226">hello VPN client has connected toohello Azure virtual network.</span></span> <span data-ttu-id="4934e-227">Hello ügyfél azonban nem tud hozzáférni hálózati megosztások.</span><span class="sxs-lookup"><span data-stu-id="4934e-227">However, hello client cannot access network shares.</span></span>

### <a name="cause"></a><span data-ttu-id="4934e-228">Ok</span><span class="sxs-lookup"><span data-stu-id="4934e-228">Cause</span></span>

<span data-ttu-id="4934e-229">fájlmegosztási hozzáférést hello SMB protokoll használható.</span><span class="sxs-lookup"><span data-stu-id="4934e-229">hello SMB protocol is used for file share access.</span></span> <span data-ttu-id="4934e-230">Hello kapcsolat elindításakor hello VPN-ügyfél hozzáadása hello munkamenet hitelesítő adatokat, és hello hiba történik.</span><span class="sxs-lookup"><span data-stu-id="4934e-230">When hello connection is initiated, hello VPN client adds hello session credentials and hello failure occurs.</span></span> <span data-ttu-id="4934e-231">Hello kapcsolat létrejötte után hello ügyfél kényszeríti toouse hello gyorsítótár Kerberos-hitelesítés hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="4934e-231">After hello connection is established, hello client is forced toouse hello cache credentials for Kerberos authentication.</span></span> <span data-ttu-id="4934e-232">A folyamat elindul a lekérdezések toohello kulcsszolgáltató (tartományvezérlő) tooget jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="4934e-232">This process initiates queries toohello Key Distribution Center (a domain controller) tooget a token.</span></span> <span data-ttu-id="4934e-233">Mivel hello ügyfél csatlakozik az internethez hello, nem feltétlenül tudja tooreach hello tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="4934e-233">Because hello client connects from hello Internet, it might not be able tooreach hello domain controller.</span></span> <span data-ttu-id="4934e-234">Ezért hello ügyfél nem feladatátvételt a Kerberos tooNTLM.</span><span class="sxs-lookup"><span data-stu-id="4934e-234">Therefore, hello client cannot fail over from Kerberos tooNTLM.</span></span> 

<span data-ttu-id="4934e-235">hello csak időt, hogy az ügyfél hello kéri a hitelesítő adatok esetén érvényes tanúsítvánnyal rendelkezik (tárolóhálózathoz = UPN) kiállító hello tartomány toowhich tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="4934e-235">hello only time that hello client is prompted for a credential is when it has a valid certificate (with SAN=UPN) issued by hello domain toowhich it is joined.</span></span> <span data-ttu-id="4934e-236">hello ügyfél is fizikailag csatlakoztatott toohello tartományi hálózaton kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4934e-236">hello client also must be physically connected toohello domain network.</span></span> <span data-ttu-id="4934e-237">Ebben az esetben hello ügyfél megpróbál toouse hello tanúsítványt, és egészítse ki toohello tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="4934e-237">In this case, hello client tries toouse hello certificate and reaches out toohello domain controller.</span></span> <span data-ttu-id="4934e-238">Hello kulcsszolgáltató majd egy "KDC_ERR_C_PRINCIPAL_UNKNOWN" hibaüzenetet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="4934e-238">Then hello Key Distribution Center returns a "KDC_ERR_C_PRINCIPAL_UNKNOWN" error.</span></span> <span data-ttu-id="4934e-239">hello ügyfél kényszerített toofail tooNTLM felett.</span><span class="sxs-lookup"><span data-stu-id="4934e-239">hello client is forced toofail over tooNTLM.</span></span> 

### <a name="solution"></a><span data-ttu-id="4934e-240">Megoldás</span><span class="sxs-lookup"><span data-stu-id="4934e-240">Solution</span></span>

<span data-ttu-id="4934e-241">toowork hello a probléma, tiltsa le a tartományi hitelesítő adatait a következő beállításkulcsot hello gyorsítótárazását hello:</span><span class="sxs-lookup"><span data-stu-id="4934e-241">toowork around hello problem, disable hello caching of domain credentials from hello following registry subkey:</span></span> 

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\DisableDomainCreds - Set hello value too1 


## <a name="cannot-find-hello-point-to-site-vpn-connection-in-windows-after-reinstalling-hello-vpn-client"></a><span data-ttu-id="4934e-242">Nem található a Windows hello pont-pont VPN-kapcsolat hello VPN-ügyfél újratelepítése után</span><span class="sxs-lookup"><span data-stu-id="4934e-242">Cannot find hello point-to-site VPN connection in Windows after reinstalling hello VPN client</span></span>

### <a name="symptom"></a><span data-ttu-id="4934e-243">Jelenség</span><span class="sxs-lookup"><span data-stu-id="4934e-243">Symptom</span></span>

<span data-ttu-id="4934e-244">Távolítsa el a hello pont-pont VPN-kapcsolatot, és telepítse a hello VPN-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="4934e-244">You remove hello point-to-site VPN connection and then reinstall hello VPN client.</span></span> <span data-ttu-id="4934e-245">Ebben a helyzetben hello VPN-kapcsolat beállítása nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="4934e-245">In this situation, hello VPN connection is not configured successfully.</span></span> <span data-ttu-id="4934e-246">Nem látja hello VPN-kapcsolatot a hello **hálózati kapcsolat** a Windows beállításai.</span><span class="sxs-lookup"><span data-stu-id="4934e-246">You do not see hello VPN connection in hello **Network connections** settings in Windows.</span></span>

### <a name="solution"></a><span data-ttu-id="4934e-247">Megoldás</span><span class="sxs-lookup"><span data-stu-id="4934e-247">Solution</span></span>

<span data-ttu-id="4934e-248">tooresolve hello problémát, törölje a hello régi VPN ügyfél konfigurációs fájlokat a **C:\Users\TheUserName\AppData\Roaming\Microsoft\Network\Connections**, majd futtassa újra a hello VPN-ügyfél telepítőjét.</span><span class="sxs-lookup"><span data-stu-id="4934e-248">tooresolve hello problem, delete hello old VPN client configuration files from **C:\Users\TheUserName\AppData\Roaming\Microsoft\Network\Connections**, and then run hello VPN client installer again.</span></span>
