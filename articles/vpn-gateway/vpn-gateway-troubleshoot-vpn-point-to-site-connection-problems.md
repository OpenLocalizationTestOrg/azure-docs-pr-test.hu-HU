---
title: "Az Azure pont – hely kapcsolat kapcsolatos problémák elhárítása |} Microsoft Docs"
description: "Útmutató a pont-pont csatlakozási hibák elhárítása."
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
ms.openlocfilehash: de37c8ffd47a2b8e201d18e3a20b5325d528ad59
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshooting-azure-point-to-site-connection-problems"></a><span data-ttu-id="0b6df-103">Hibáinak elhárítása: Az Azure pont – hely kapcsolat problémák</span><span class="sxs-lookup"><span data-stu-id="0b6df-103">Troubleshooting: Azure point-to-site connection problems</span></span>

<span data-ttu-id="0b6df-104">A cikk ismerteti, amelyekkel Ön is szembesülhet pont-pont csatlakozási probléma.</span><span class="sxs-lookup"><span data-stu-id="0b6df-104">This article lists common point-to-site connection problems that you might experience.</span></span> <span data-ttu-id="0b6df-105">Lehetséges okait és megoldásait ezeket a problémákat is ismerteti.</span><span class="sxs-lookup"><span data-stu-id="0b6df-105">It also discusses possible causes and solutions for these problems.</span></span>

## <a name="vpn-client-error-a-certificate-could-not-be-found"></a><span data-ttu-id="0b6df-106">VPN-ügyfél hiba: A tanúsítvány nem található.</span><span class="sxs-lookup"><span data-stu-id="0b6df-106">VPN client error: A certificate could not be found</span></span>

### <a name="symptom"></a><span data-ttu-id="0b6df-107">Jelenség</span><span class="sxs-lookup"><span data-stu-id="0b6df-107">Symptom</span></span>

<span data-ttu-id="0b6df-108">A VPN-ügyfél használatával csatlakoznak az Azure virtuális hálózat megkísérlésekor a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="0b6df-108">When you try to connect to an Azure virtual network by using the VPN client, you receive the following error message:</span></span>

<span data-ttu-id="0b6df-109">**A tanúsítvány nem található, amely használható a bővíthető hitelesítési protokoll. (798 hiba)**</span><span class="sxs-lookup"><span data-stu-id="0b6df-109">**A certificate could not be found that can be used with this Extensible Authentication Protocol. (Error 798)**</span></span>

### <a name="cause"></a><span data-ttu-id="0b6df-110">Ok</span><span class="sxs-lookup"><span data-stu-id="0b6df-110">Cause</span></span>

<span data-ttu-id="0b6df-111">Ez a probléma akkor fordul elő, ha az ügyfél-tanúsítványa nem található a **tanúsítványok - aktuális User\Personal\Certificates**.</span><span class="sxs-lookup"><span data-stu-id="0b6df-111">This problem occurs if the client certificate is missing from **Certificates - Current User\Personal\Certificates**.</span></span>

### <a name="solution"></a><span data-ttu-id="0b6df-112">Megoldás</span><span class="sxs-lookup"><span data-stu-id="0b6df-112">Solution</span></span>

<span data-ttu-id="0b6df-113">Győződjön meg arról, hogy az ügyféltanúsítvány telepítve van-e a tanúsítványok tároló (Certmgr.msc) a következő helyen:</span><span class="sxs-lookup"><span data-stu-id="0b6df-113">Make sure that the client certificate is installed in the following location of the Certificates store (Certmgr.msc):</span></span>
 
<span data-ttu-id="0b6df-114">**Tanúsítványok – aktuális User\Personal\Certificates**</span><span class="sxs-lookup"><span data-stu-id="0b6df-114">**Certificates - Current User\Personal\Certificates**</span></span>

<span data-ttu-id="0b6df-115">Az ügyféltanúsítvány telepítésével kapcsolatos további információkért lásd: [tanúsítvány létrehozása és exportálása a pont – hely kapcsolatok](vpn-gateway-certificates-point-to-site.md).</span><span class="sxs-lookup"><span data-stu-id="0b6df-115">For more information about how to install the client certificate, see [Generate and export certificates for point-to-site connections](vpn-gateway-certificates-point-to-site.md).</span></span>

> [!NOTE]
> <span data-ttu-id="0b6df-116">Amikor importálja az ügyféltanúsítványt, ne jelölje be a **titkos kulcs erős védelmének engedélyezése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="0b6df-116">When you import the client certificate, do not select the **Enable strong private key protection** option.</span></span>

## <a name="vpn-client-error-the-message-received-was-unexpected-or-badly-formatted"></a><span data-ttu-id="0b6df-117">VPN-ügyfél hiba: A fogadott üzenet nem várt vagy rosszul formázott</span><span class="sxs-lookup"><span data-stu-id="0b6df-117">VPN client error: The message received was unexpected or badly formatted</span></span>

### <a name="symptom"></a><span data-ttu-id="0b6df-118">Jelenség</span><span class="sxs-lookup"><span data-stu-id="0b6df-118">Symptom</span></span>

<span data-ttu-id="0b6df-119">A VPN-ügyfél használatával csatlakoznak az Azure virtuális hálózat megkísérlésekor a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="0b6df-119">When you try to connect to an Azure virtual network by using the VPN client, you receive the following error message:</span></span>

<span data-ttu-id="0b6df-120">**A fogadott üzenet volt-e váratlan vagy rosszul formázott. (0x80090326 hiba)**</span><span class="sxs-lookup"><span data-stu-id="0b6df-120">**The message received was unexpected or badly formatted. (Error 0x80090326)**</span></span>

### <a name="cause"></a><span data-ttu-id="0b6df-121">Ok</span><span class="sxs-lookup"><span data-stu-id="0b6df-121">Cause</span></span>

<span data-ttu-id="0b6df-122">Ez a probléma akkor fordul elő, ha a legfelső szintű tanúsítvány nyilvános kulcsa nem van töltve az Azure VPN gateway.</span><span class="sxs-lookup"><span data-stu-id="0b6df-122">This problem occurs if the root certificate public key is not uploaded into the Azure VPN gateway.</span></span> <span data-ttu-id="0b6df-123">Ez akkor is előfordulhat, ha a kulcs sérült vagy lejárt.</span><span class="sxs-lookup"><span data-stu-id="0b6df-123">It can also occur if the key is corrupted or expired.</span></span>

### <a name="solution"></a><span data-ttu-id="0b6df-124">Megoldás</span><span class="sxs-lookup"><span data-stu-id="0b6df-124">Solution</span></span>

<span data-ttu-id="0b6df-125">A probléma megoldásához, a legfelső szintű tanúsítvány megtekintéséhez, hogy azt vissza lett vonva az Azure portálon állapotának ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="0b6df-125">To resolve this problem, check the status of the root certificate in the Azure portal to see whether it was revoked.</span></span> <span data-ttu-id="0b6df-126">Ha nincs visszavonva, próbálja meg törölni a főtanúsítványt és reupload.</span><span class="sxs-lookup"><span data-stu-id="0b6df-126">If it is not revoked, try to delete the root certificate and reupload.</span></span> <span data-ttu-id="0b6df-127">További információkért lásd: [olyan tanúsítványokat hoznak létre](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).</span><span class="sxs-lookup"><span data-stu-id="0b6df-127">For more information, see [Create certificates](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).</span></span>

## <a name="vpn-client-error-a-certificate-chain-processed-but-terminated"></a><span data-ttu-id="0b6df-128">VPN-ügyfél hiba: A tanúsítványlánc feldolgozása, de a megszakadt</span><span class="sxs-lookup"><span data-stu-id="0b6df-128">VPN client error: A certificate chain processed but terminated</span></span> 

### <a name="symptom"></a><span data-ttu-id="0b6df-129">Jelenség</span><span class="sxs-lookup"><span data-stu-id="0b6df-129">Symptom</span></span> 

<span data-ttu-id="0b6df-130">A VPN-ügyfél használatával csatlakoznak az Azure virtuális hálózat megkísérlésekor a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="0b6df-130">When you try to connect to an Azure virtual network by using the VPN client, you receive the following error message:</span></span>

<span data-ttu-id="0b6df-131">**A tanúsítványlánc feldolgozása, de a legfelső szintű tanúsítvány nem bízik meg a megbízható szolgáltatót megszakadt.**</span><span class="sxs-lookup"><span data-stu-id="0b6df-131">**A certificate chain processed but terminated in a root certificate which is not trusted by the trust provider.**</span></span>

### <a name="solution"></a><span data-ttu-id="0b6df-132">Megoldás</span><span class="sxs-lookup"><span data-stu-id="0b6df-132">Solution</span></span>

1. <span data-ttu-id="0b6df-133">Győződjön meg arról, hogy az alábbi tanúsítványok vannak-e a megfelelő helyen:</span><span class="sxs-lookup"><span data-stu-id="0b6df-133">Make sure that the following certificates are in the correct location:</span></span>

    | <span data-ttu-id="0b6df-134">Tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="0b6df-134">Certificate</span></span> | <span data-ttu-id="0b6df-135">Hely</span><span class="sxs-lookup"><span data-stu-id="0b6df-135">Location</span></span> |
    | ------------- | ------------- |
    | <span data-ttu-id="0b6df-136">AzureClient.pfx</span><span class="sxs-lookup"><span data-stu-id="0b6df-136">AzureClient.pfx</span></span>  | <span data-ttu-id="0b6df-137">Aktuális User\Personal\Certificates</span><span class="sxs-lookup"><span data-stu-id="0b6df-137">Current User\Personal\Certificates</span></span> |
    | <span data-ttu-id="0b6df-138">Azuregateway -*GUID*. cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="0b6df-138">Azuregateway-*GUID*.cloudapp.net</span></span>  | <span data-ttu-id="0b6df-139">Aktuális User\Trusted legfelső szintű hitelesítésszolgáltatók</span><span class="sxs-lookup"><span data-stu-id="0b6df-139">Current User\Trusted Root Certification Authorities</span></span>|
    | <span data-ttu-id="0b6df-140">AzureGateway -*GUID*. cloudapp.net, AzureRoot.cer</span><span class="sxs-lookup"><span data-stu-id="0b6df-140">AzureGateway-*GUID*.cloudapp.net, AzureRoot.cer</span></span>    | <span data-ttu-id="0b6df-141">Helyi számítógép\Megbízható legfelső szintű hitelesítésszolgáltatók</span><span class="sxs-lookup"><span data-stu-id="0b6df-141">Local Computer\Trusted Root Certification Authorities</span></span>|

2. <span data-ttu-id="0b6df-142">Ha a tanúsítvány már a helyen, próbálja meg törölni a tanúsítványokat, és telepítse újra.</span><span class="sxs-lookup"><span data-stu-id="0b6df-142">If the certificates are already in the location, try to delete the certificates and reinstall them.</span></span> <span data-ttu-id="0b6df-143">A  **azuregateway -*GUID*. az ügyfél VPN-konfiguráció Azure-portálról letöltött csomag cloudapp.net** tanúsítvány van.</span><span class="sxs-lookup"><span data-stu-id="0b6df-143">The **azuregateway-*GUID*.cloudapp.net** certificate is in the VPN client configuration package that you downloaded from the Azure portal.</span></span> <span data-ttu-id="0b6df-144">Fájl archivers segítségével csomagolja ki a fájlokat a csomagból.</span><span class="sxs-lookup"><span data-stu-id="0b6df-144">You can use file archivers to extract the files from the package.</span></span>

## <a name="file-download-error-target-uri-is-not-specified"></a><span data-ttu-id="0b6df-145">Letöltési hiba: nincs megadva a cél URI Azonosítóját</span><span class="sxs-lookup"><span data-stu-id="0b6df-145">File download error: Target URI is not specified</span></span>

### <a name="symptom"></a><span data-ttu-id="0b6df-146">Jelenség</span><span class="sxs-lookup"><span data-stu-id="0b6df-146">Symptom</span></span>

<span data-ttu-id="0b6df-147">A következő hibaüzenetet kapja:</span><span class="sxs-lookup"><span data-stu-id="0b6df-147">You receive the following error message:</span></span>

<span data-ttu-id="0b6df-148">**Hiba történt a fájl letöltése. Nincs megadva a cél URI Azonosítóját.**</span><span class="sxs-lookup"><span data-stu-id="0b6df-148">**File download error. Target URI is not specified.**</span></span>

### <a name="cause"></a><span data-ttu-id="0b6df-149">Ok</span><span class="sxs-lookup"><span data-stu-id="0b6df-149">Cause</span></span> 

<span data-ttu-id="0b6df-150">A probléma miatt egy hibás átjáró típusa.</span><span class="sxs-lookup"><span data-stu-id="0b6df-150">This problem occurs because of an incorrect gateway type.</span></span> 

### <a name="solution"></a><span data-ttu-id="0b6df-151">Megoldás</span><span class="sxs-lookup"><span data-stu-id="0b6df-151">Solution</span></span>

<span data-ttu-id="0b6df-152">A VPN-átjáró típusúnak kell lennie **VPN**, és a VPN-típus lehet **RouteBased**.</span><span class="sxs-lookup"><span data-stu-id="0b6df-152">The VPN gateway type must be **VPN**, and the VPN type must be **RouteBased**.</span></span>

## <a name="vpn-client-error-azure-vpn-custom-script-failed"></a><span data-ttu-id="0b6df-153">VPN-ügyfél hiba: az Azure VPN egyéni parancsprogram végrehajtása sikertelen volt</span><span class="sxs-lookup"><span data-stu-id="0b6df-153">VPN client error: Azure VPN custom script failed</span></span> 

### <a name="symptom"></a><span data-ttu-id="0b6df-154">Jelenség</span><span class="sxs-lookup"><span data-stu-id="0b6df-154">Symptom</span></span>

<span data-ttu-id="0b6df-155">A VPN-ügyfél használatával csatlakoznak az Azure virtuális hálózat megkísérlésekor a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="0b6df-155">When you try to connect to an Azure virtual network by using the VPN client, you receive the following error message:</span></span>

<span data-ttu-id="0b6df-156">**Egyéni parancsfájl (frissítés az útválasztási táblában) sikertelen volt. (8007026f hiba)**</span><span class="sxs-lookup"><span data-stu-id="0b6df-156">**Custom script (to update your routing table) failed. (Error 8007026f)**</span></span>

### <a name="cause"></a><span data-ttu-id="0b6df-157">Ok</span><span class="sxs-lookup"><span data-stu-id="0b6df-157">Cause</span></span>

<span data-ttu-id="0b6df-158">Ez a probléma akkor fordulhat elő, ha a webhely pont közötti VPN-kapcsolat megnyitása egy helyi használatával próbálja.</span><span class="sxs-lookup"><span data-stu-id="0b6df-158">This problem might occur if you are trying to open the site-to-point VPN connection by using a shortcut.</span></span>

### <a name="solution"></a><span data-ttu-id="0b6df-159">Megoldás</span><span class="sxs-lookup"><span data-stu-id="0b6df-159">Solution</span></span> 

<span data-ttu-id="0b6df-160">Nyissa meg a VPN-csomag közvetlenül helyett a helyi megnyitja azt.</span><span class="sxs-lookup"><span data-stu-id="0b6df-160">Open the VPN package directly instead of opening it from the shortcut.</span></span>

## <a name="cannot-install-the-vpn-client"></a><span data-ttu-id="0b6df-161">A VPN-ügyfél nem tudja telepíteni.</span><span class="sxs-lookup"><span data-stu-id="0b6df-161">Cannot install the VPN client</span></span>

### <a name="cause"></a><span data-ttu-id="0b6df-162">Ok</span><span class="sxs-lookup"><span data-stu-id="0b6df-162">Cause</span></span> 

<span data-ttu-id="0b6df-163">Kiegészítő igazolás hogy bízzon meg a VPN-átjáró a virtuális hálózat szükséges.</span><span class="sxs-lookup"><span data-stu-id="0b6df-163">An additional certificate is required to trust the VPN gateway for your virtual network.</span></span> <span data-ttu-id="0b6df-164">A tanúsítványt a VPN-konfigurációs ügyfélcsomag, az Azure-portálon létrehozott tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0b6df-164">The certificate is included in the VPN client configuration package that is generated from the Azure portal.</span></span>

### <a name="solution"></a><span data-ttu-id="0b6df-165">Megoldás</span><span class="sxs-lookup"><span data-stu-id="0b6df-165">Solution</span></span>

<span data-ttu-id="0b6df-166">Bontsa ki a VPN-ügyfélcsomag konfigurációs, és keresse meg a .cer fájlt.</span><span class="sxs-lookup"><span data-stu-id="0b6df-166">Extract the VPN client configuration package, and find the .cer file.</span></span> <span data-ttu-id="0b6df-167">A tanúsítvány telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0b6df-167">To install the certificate, follow these steps:</span></span>

1. <span data-ttu-id="0b6df-168">Nyissa meg a mmc.exe.</span><span class="sxs-lookup"><span data-stu-id="0b6df-168">Open mmc.exe.</span></span>
2. <span data-ttu-id="0b6df-169">Adja hozzá a **tanúsítványok** beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="0b6df-169">Add the **Certificates** snap-in.</span></span>
3. <span data-ttu-id="0b6df-170">Válassza ki a **számítógép** fiókot a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0b6df-170">Select the **Computer** account for the local computer.</span></span>
4. <span data-ttu-id="0b6df-171">Kattintson a jobb gombbal a **megbízható legfelső szintű hitelesítésszolgáltatók** csomópont.</span><span class="sxs-lookup"><span data-stu-id="0b6df-171">Right-click the **Trusted Root Certification Authorities** node.</span></span> <span data-ttu-id="0b6df-172">Kattintson a **minden-tevékenység** > **importálási**, és keresse meg a .cer fájlt, a VPN-ügyfélcsomag konfigurációs kibontott.</span><span class="sxs-lookup"><span data-stu-id="0b6df-172">Click **All-Task** > **Import**, and browse to the .cer file you extracted from the VPN client configuration package.</span></span>
5. <span data-ttu-id="0b6df-173">Indítsa újra a számítógépet.</span><span class="sxs-lookup"><span data-stu-id="0b6df-173">Restart the computer.</span></span> 
6. <span data-ttu-id="0b6df-174">Próbálja meg telepíteni a VPN-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="0b6df-174">Try to install the VPN client.</span></span>

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-data-is-invalid"></a><span data-ttu-id="0b6df-175">Az Azure portál hiba: nem sikerült menteni a VPN-átjáró, és az adat érvénytelen</span><span class="sxs-lookup"><span data-stu-id="0b6df-175">Azure portal error: Failed to save the VPN gateway, and the data is invalid</span></span>

### <a name="symptom"></a><span data-ttu-id="0b6df-176">Jelenség</span><span class="sxs-lookup"><span data-stu-id="0b6df-176">Symptom</span></span>

<span data-ttu-id="0b6df-177">A VPN-átjáró módosításainak mentése az Azure-portálon megkísérlésekor a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="0b6df-177">When you try to save the changes for the VPN gateway in the Azure portal, you receive the following error message:</span></span>

<span data-ttu-id="0b6df-178">**Nem sikerült menteni a virtuális hálózati átjáró &lt;* átjárónevet*&gt;.</span><span class="sxs-lookup"><span data-stu-id="0b6df-178">**Failed to save virtual network gateway &lt;*gateway name*&gt;.</span></span> <span data-ttu-id="0b6df-179">Tanúsítvány adatainak &lt; *tanúsítvány azonosító* &gt; van invalid.* *</span><span class="sxs-lookup"><span data-stu-id="0b6df-179">Data for certificate &lt;*certificate ID*&gt; is invalid.**</span></span>

### <a name="cause"></a><span data-ttu-id="0b6df-180">Ok</span><span class="sxs-lookup"><span data-stu-id="0b6df-180">Cause</span></span> 

<span data-ttu-id="0b6df-181">Ez a probléma akkor fordulhat elő, ha a legfelső szintű tanúsítvány nyilvános kulcsa feltöltött tartalmaz egy érvénytelen karakter, például egy szóközzel.</span><span class="sxs-lookup"><span data-stu-id="0b6df-181">This problem might occur if the root certificate public key that you uploaded contains an invalid character, such as a space.</span></span>

### <a name="solution"></a><span data-ttu-id="0b6df-182">Megoldás</span><span class="sxs-lookup"><span data-stu-id="0b6df-182">Solution</span></span>

<span data-ttu-id="0b6df-183">Győződjön meg arról, hogy az adatok a tanúsítvány nem tartalmaz érvénytelen karaktereket, például sortörést (kocsivissza).</span><span class="sxs-lookup"><span data-stu-id="0b6df-183">Make sure that the data in the certificate does not contain invalid characters, such as line breaks (carriage returns).</span></span> <span data-ttu-id="0b6df-184">Az egész értéknek kell lennie egy hosszú sor.</span><span class="sxs-lookup"><span data-stu-id="0b6df-184">The entire value should be one long line.</span></span> <span data-ttu-id="0b6df-185">A következő szöveg látható egy minta a tanúsítványt:</span><span class="sxs-lookup"><span data-stu-id="0b6df-185">The following text is a sample of the certificate:</span></span>

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

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-resource-name-is-invalid"></a><span data-ttu-id="0b6df-186">Az Azure portál hiba: nem sikerült menteni a VPN-átjáró, és az erőforrás neve érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="0b6df-186">Azure portal error: Failed to save the VPN gateway, and the resource name is invalid</span></span>

### <a name="symptom"></a><span data-ttu-id="0b6df-187">Jelenség</span><span class="sxs-lookup"><span data-stu-id="0b6df-187">Symptom</span></span>

<span data-ttu-id="0b6df-188">A VPN-átjáró módosításainak mentése az Azure-portálon megkísérlésekor a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="0b6df-188">When you try to save the changes for the VPN gateway in the Azure portal, you receive the following error message:</span></span> 

<span data-ttu-id="0b6df-189">**Nem sikerült menteni a virtuális hálózati átjáró &lt;* átjárónevet*&gt;.</span><span class="sxs-lookup"><span data-stu-id="0b6df-189">**Failed to save virtual network gateway &lt;*gateway name*&gt;.</span></span> <span data-ttu-id="0b6df-190">Az erőforrásnév &lt; *megpróbálja feltölteni a tanúsítvány neve* &gt; van érvénytelen **.</span><span class="sxs-lookup"><span data-stu-id="0b6df-190">Resource name &lt;*certificate name you try to upload*&gt; is invalid**.</span></span>

### <a name="cause"></a><span data-ttu-id="0b6df-191">Ok</span><span class="sxs-lookup"><span data-stu-id="0b6df-191">Cause</span></span>

<span data-ttu-id="0b6df-192">A probléma oka, hogy a tanúsítvány neve érvénytelen karaktert, például a szóközt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0b6df-192">This problem occurs because the name of the certificate contains an invalid character, such as a space.</span></span> 

## <a name="azure-portal-error-vpn-package-file-download-error-503"></a><span data-ttu-id="0b6df-193">Az Azure portál hiba: VPN csomag fájl letöltési hiba 503-as</span><span class="sxs-lookup"><span data-stu-id="0b6df-193">Azure portal error: VPN package file download error 503</span></span>

### <a name="symptom"></a><span data-ttu-id="0b6df-194">Jelenség</span><span class="sxs-lookup"><span data-stu-id="0b6df-194">Symptom</span></span>

<span data-ttu-id="0b6df-195">Töltse le a VPN-ügyfélcsomag konfigurációs megkísérlésekor a következő hibaüzenet jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="0b6df-195">When you try to download the VPN client configuration package, you receive the following error message:</span></span>

<span data-ttu-id="0b6df-196">**Nem sikerült letölteni a fájlt. Hiba legutolsó részletes adatai: 503-as hiba. A kiszolgáló túlterhelt.**</span><span class="sxs-lookup"><span data-stu-id="0b6df-196">**Failed to download the file. Error details: error 503. The server is busy.**</span></span>
 
### <a name="solution"></a><span data-ttu-id="0b6df-197">Megoldás</span><span class="sxs-lookup"><span data-stu-id="0b6df-197">Solution</span></span>

<span data-ttu-id="0b6df-198">Ez a hiba átmeneti hálózati probléma okozhatja.</span><span class="sxs-lookup"><span data-stu-id="0b6df-198">This error can be caused by a temporary network problem.</span></span> <span data-ttu-id="0b6df-199">Próbálkozzon újra néhány perc múlva a VPN-csomagjának letöltése.</span><span class="sxs-lookup"><span data-stu-id="0b6df-199">Try to download the VPN package again after a few minutes.</span></span>

## <a name="azure-vpn-gateway-upgrade-all-p2s-clients-are-unable-to-connect"></a><span data-ttu-id="0b6df-200">Az Azure VPN Gateway frissítése: minden P2S-ügyfelek nem tudnak csatlakozni</span><span class="sxs-lookup"><span data-stu-id="0b6df-200">Azure VPN Gateway upgrade: All P2S clients are unable to connect</span></span>

### <a name="cause"></a><span data-ttu-id="0b6df-201">Ok</span><span class="sxs-lookup"><span data-stu-id="0b6df-201">Cause</span></span>

<span data-ttu-id="0b6df-202">Ha a tanúsítvány több mint 50 %-a keresztül élettartamuk, a tanúsítvány frissítése.</span><span class="sxs-lookup"><span data-stu-id="0b6df-202">If the certificate is more than 50 percent through its lifetime, the certificate is rolled over.</span></span>

### <a name="solution"></a><span data-ttu-id="0b6df-203">Megoldás</span><span class="sxs-lookup"><span data-stu-id="0b6df-203">Solution</span></span>

<span data-ttu-id="0b6df-204">Ez a probléma megoldása érdekében hozzon létre, és újra elosztják a VPN-ügyfelek az új tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="0b6df-204">To resolve this problem, create and redistribute new certificates to the VPN clients.</span></span> 

## <a name="too-many-vpn-clients-connected-at-once"></a><span data-ttu-id="0b6df-205">Túl sok a VPN-ügyfelek egyszerre csatlakoztatva</span><span class="sxs-lookup"><span data-stu-id="0b6df-205">Too many VPN clients connected at once</span></span>

<span data-ttu-id="0b6df-206">Minden egyes VPN-átjáró esetén engedélyezett kapcsolatok maximális száma 128.</span><span class="sxs-lookup"><span data-stu-id="0b6df-206">For each VPN gateway, the maximum number of allowable connections is 128.</span></span> <span data-ttu-id="0b6df-207">Láthatja, hogy az Azure portálon csatlakoztatott ügyfelek teljes száma.</span><span class="sxs-lookup"><span data-stu-id="0b6df-207">You can see the total number of connected clients in the Azure portal.</span></span>

## <a name="point-to-site-vpn-incorrectly-adds-a-route-for-100008-to-the-route-table"></a><span data-ttu-id="0b6df-208">Pont – hely típusú VPN helytelenül 10.0.0.0/8 útvonal hozzáadása az útvonaltábla</span><span class="sxs-lookup"><span data-stu-id="0b6df-208">Point-to-site VPN incorrectly adds a route for 10.0.0.0/8 to the route table</span></span>

### <a name="symptom"></a><span data-ttu-id="0b6df-209">Jelenség</span><span class="sxs-lookup"><span data-stu-id="0b6df-209">Symptom</span></span>

<span data-ttu-id="0b6df-210">Amikor a VPN-kapcsolatot a pont-pont ügyfélen, a VPN-ügyfél az Azure virtuális hálózat felé útvonal kell hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="0b6df-210">When you dial the VPN connection on the point-to-site client, the VPN client should add a route toward the Azure virtual network.</span></span> <span data-ttu-id="0b6df-211">Az IP-segítő szolgáltatás hozzá kell adnia egy útvonalat a VPN-ügyfelek az alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="0b6df-211">The IP helper service should add a route for the subnet of the VPN clients.</span></span> 

<span data-ttu-id="0b6df-212">A VPN-ügyfél tartomány 10.0.0.0/8, például a 10.0.12.0/24 kisebb alhálózatának tartozik.</span><span class="sxs-lookup"><span data-stu-id="0b6df-212">The VPN client range belongs to a smaller subnet of 10.0.0.0/8, such as 10.0.12.0/24.</span></span> <span data-ttu-id="0b6df-213">Helyett 10.0.12.0/24 útvonal, a 10.0.0.0/8 adjon hozzá egy útvonalat magasabb prioritással bír, amely.</span><span class="sxs-lookup"><span data-stu-id="0b6df-213">Instead of a route for 10.0.12.0/24, a route for 10.0.0.0/8 is added that has higher priority.</span></span> 

<span data-ttu-id="0b6df-214">Ez helytelen az útvonal más hálózatokkal helyszíni, amely lehet, hogy a 10.0.0.0/8 tartományba 10.50.0.0/24, például egy másik alhálózat tartozik, amelyek nem rendelkeznek egy adott útvonal definiálva kapcsolat megszakad.</span><span class="sxs-lookup"><span data-stu-id="0b6df-214">This incorrect route breaks connectivity with other on-premises networks that might belong to another subnet within the 10.0.0.0/8 range, such as 10.50.0.0/24, that don't have a specific route defined.</span></span> 

### <a name="cause"></a><span data-ttu-id="0b6df-215">Ok</span><span class="sxs-lookup"><span data-stu-id="0b6df-215">Cause</span></span>

<span data-ttu-id="0b6df-216">Ez szándékosan van a Windows-ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="0b6df-216">This behavior is by design for Windows clients.</span></span> <span data-ttu-id="0b6df-217">Amikor az ügyfél a PPP IPCP protokollt használja, az IP-címet a alagútkapcsolat megszerzi a kiszolgálóról (a jelen esetben a VPN-átjáró).</span><span class="sxs-lookup"><span data-stu-id="0b6df-217">When the client uses the PPP IPCP protocol, it obtains the IP address for the tunnel interface from the server (the VPN gateway in this case).</span></span> <span data-ttu-id="0b6df-218">Azonban a protokoll korlátozása, mert az ügyfél nem rendelkezik az alhálózati maszkot.</span><span class="sxs-lookup"><span data-stu-id="0b6df-218">However, because of a limitation in the protocol, the client does not have the subnet mask.</span></span> <span data-ttu-id="0b6df-219">Mivel más módon nem lehet legyen, az ügyfél megpróbálja kitalálni az alhálózati maszkot az osztály az alagutat illesztő IP-cím alapján.</span><span class="sxs-lookup"><span data-stu-id="0b6df-219">Because there is no other way to get it, the client tries to guess the subnet mask based on the class of the tunnel interface IP address.</span></span> 

<span data-ttu-id="0b6df-220">Ezért adjon hozzá egy útvonalat a következő statikus hozzárendelés alapján:</span><span class="sxs-lookup"><span data-stu-id="0b6df-220">Therefore, a route is added based on the following static mapping:</span></span> 

<span data-ttu-id="0b6df-221">Ha a cím az Öné egy--> alkalmazása /8</span><span class="sxs-lookup"><span data-stu-id="0b6df-221">If address belongs to class A --> apply /8</span></span>

<span data-ttu-id="0b6df-222">Ha a cím az Öné B--> osztály /16 alkalmazása</span><span class="sxs-lookup"><span data-stu-id="0b6df-222">If address belongs to class B --> apply /16</span></span>

<span data-ttu-id="0b6df-223">Ha a cím az Öné C--> osztály /24 alkalmazása</span><span class="sxs-lookup"><span data-stu-id="0b6df-223">If address belongs to class C --> apply /24</span></span>

## <a name="vpn-client-cannot-access-network-file-shares"></a><span data-ttu-id="0b6df-224">VPN-ügyfél nem tud hozzáférni a hálózati fájlmegosztások</span><span class="sxs-lookup"><span data-stu-id="0b6df-224">VPN client cannot access network file shares</span></span>

### <a name="symptom"></a><span data-ttu-id="0b6df-225">Jelenség</span><span class="sxs-lookup"><span data-stu-id="0b6df-225">Symptom</span></span>

<span data-ttu-id="0b6df-226">A VPN-ügyfél csatlakozott az Azure virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="0b6df-226">The VPN client has connected to the Azure virtual network.</span></span> <span data-ttu-id="0b6df-227">Az ügyfél nem érhető el, hálózati megosztások.</span><span class="sxs-lookup"><span data-stu-id="0b6df-227">However, the client cannot access network shares.</span></span>

### <a name="cause"></a><span data-ttu-id="0b6df-228">Ok</span><span class="sxs-lookup"><span data-stu-id="0b6df-228">Cause</span></span>

<span data-ttu-id="0b6df-229">Az SMB protokollt használják a fájlmegosztási hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="0b6df-229">The SMB protocol is used for file share access.</span></span> <span data-ttu-id="0b6df-230">Amikor a rendszer kezdeményezi a kapcsolatot, a VPN-ügyfél hozzáadása a munkamenet hitelesítő adatokat, és a hiba akkor fordul elő.</span><span class="sxs-lookup"><span data-stu-id="0b6df-230">When the connection is initiated, the VPN client adds the session credentials and the failure occurs.</span></span> <span data-ttu-id="0b6df-231">A kapcsolat létrejötte után a rendszer kényszeríti az ügyfelet, a gyorsítótár hitelesítő adatok használata a Kerberos-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="0b6df-231">After the connection is established, the client is forced to use the cache credentials for Kerberos authentication.</span></span> <span data-ttu-id="0b6df-232">A folyamat elindul a lekérdezéseket, amelyekkel a kulcsszolgáltató (tartományvezérlő) a szolgáltatáshitelesítést egy token.</span><span class="sxs-lookup"><span data-stu-id="0b6df-232">This process initiates queries to the Key Distribution Center (a domain controller) to get a token.</span></span> <span data-ttu-id="0b6df-233">Mivel az ügyfél az internetről csatlakozik, akkor nem feltétlenül tudni érnie a tartományvezérlő.</span><span class="sxs-lookup"><span data-stu-id="0b6df-233">Because the client connects from the Internet, it might not be able to reach the domain controller.</span></span> <span data-ttu-id="0b6df-234">Ezért az ügyfél nem feladatátvételt a Kerberos NTLM.</span><span class="sxs-lookup"><span data-stu-id="0b6df-234">Therefore, the client cannot fail over from Kerberos to NTLM.</span></span> 

<span data-ttu-id="0b6df-235">Az egyetlen alkalom, hogy az ügyfélnek meg kell adnia a hitelesítő adatok megadása, ha van egy érvényes tanúsítványt (tárolóhálózathoz = UPN) kiállítja a tartomány, amelyhez csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="0b6df-235">The only time that the client is prompted for a credential is when it has a valid certificate (with SAN=UPN) issued by the domain to which it is joined.</span></span> <span data-ttu-id="0b6df-236">Az ügyfél is kell fizikailag csatlakozniuk kell a tartományi hálózaton.</span><span class="sxs-lookup"><span data-stu-id="0b6df-236">The client also must be physically connected to the domain network.</span></span> <span data-ttu-id="0b6df-237">Ebben az esetben az ügyfél megpróbálja a tanúsítványt használni, és egészítse ki a tartományvezérlőre.</span><span class="sxs-lookup"><span data-stu-id="0b6df-237">In this case, the client tries to use the certificate and reaches out to the domain controller.</span></span> <span data-ttu-id="0b6df-238">A kulcsszolgáltató majd egy "KDC_ERR_C_PRINCIPAL_UNKNOWN" hibaüzenetet adja vissza.</span><span class="sxs-lookup"><span data-stu-id="0b6df-238">Then the Key Distribution Center returns a "KDC_ERR_C_PRINCIPAL_UNKNOWN" error.</span></span> <span data-ttu-id="0b6df-239">Az ügyfél NTLM átvehetné kényszeríti.</span><span class="sxs-lookup"><span data-stu-id="0b6df-239">The client is forced to fail over to NTLM.</span></span> 

### <a name="solution"></a><span data-ttu-id="0b6df-240">Megoldás</span><span class="sxs-lookup"><span data-stu-id="0b6df-240">Solution</span></span>

<span data-ttu-id="0b6df-241">A probléma megoldása érdekében tiltsa le a gyorsítótárba helyezése tartományi hitelesítő adatait a következő beállításkulcsot:</span><span class="sxs-lookup"><span data-stu-id="0b6df-241">To work around the problem, disable the caching of domain credentials from the following registry subkey:</span></span> 

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\DisableDomainCreds - Set the value to 1 


## <a name="cannot-find-the-point-to-site-vpn-connection-in-windows-after-reinstalling-the-vpn-client"></a><span data-ttu-id="0b6df-242">Nem található a Windows a pont-pont VPN-kapcsolatot a VPN-ügyfél újratelepítése után</span><span class="sxs-lookup"><span data-stu-id="0b6df-242">Cannot find the point-to-site VPN connection in Windows after reinstalling the VPN client</span></span>

### <a name="symptom"></a><span data-ttu-id="0b6df-243">Jelenség</span><span class="sxs-lookup"><span data-stu-id="0b6df-243">Symptom</span></span>

<span data-ttu-id="0b6df-244">Távolítsa el a pont-pont VPN-kapcsolatot, és telepítse újra a VPN-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="0b6df-244">You remove the point-to-site VPN connection and then reinstall the VPN client.</span></span> <span data-ttu-id="0b6df-245">Ebben a helyzetben a VPN-kapcsolat beállítása nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="0b6df-245">In this situation, the VPN connection is not configured successfully.</span></span> <span data-ttu-id="0b6df-246">A VPN-kapcsolatot a nem látja a **hálózati kapcsolat** a Windows beállításai.</span><span class="sxs-lookup"><span data-stu-id="0b6df-246">You do not see the VPN connection in the **Network connections** settings in Windows.</span></span>

### <a name="solution"></a><span data-ttu-id="0b6df-247">Megoldás</span><span class="sxs-lookup"><span data-stu-id="0b6df-247">Solution</span></span>

<span data-ttu-id="0b6df-248">A probléma megoldása érdekében törölje a régi VPN ügyfél konfigurációs fájlokat a **C:\Users\TheUserName\AppData\Roaming\Microsoft\Network\Connections**, majd futtassa újra a VPN-ügyfél telepítőjét.</span><span class="sxs-lookup"><span data-stu-id="0b6df-248">To resolve the problem, delete the old VPN client configuration files from **C:\Users\TheUserName\AppData\Roaming\Microsoft\Network\Connections**, and then run the VPN client installer again.</span></span>
