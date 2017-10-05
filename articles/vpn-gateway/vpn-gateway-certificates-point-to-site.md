---
title: "Készítése és a pont-pont tanúsítványok exportálása: PowerShell: Azure |} Microsoft Docs"
description: "A cikkben található lépéseket hozzon létre egy önaláírt legfelső szintű tanúsítványt, a nyilvános kulcs exportálásának és a PowerShell-lel Windows 10 ügyféltanúsítványok előállításához."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 27b99f7c-50dc-4f88-8a6e-d60080819a43
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: f96b9b212b9322d0677e49ff95184d0feccca2df
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-powershell-on-windows-10"></a><span data-ttu-id="664ff-103">Létrehozása és exportálása a tanúsítványok a PowerShell-lel Windows 10 pont – hely kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="664ff-103">Generate and export certificates for Point-to-Site connections using PowerShell on Windows 10</span></span>

<span data-ttu-id="664ff-104">Pont – hely kapcsolatok tanúsítványok segítségével hitelesíti.</span><span class="sxs-lookup"><span data-stu-id="664ff-104">Point-to-Site connections use certificates to authenticate.</span></span> <span data-ttu-id="664ff-105">Ez a cikk bemutatja, hogyan hozzon létre egy önaláírt legfelső szintű tanúsítványt, és a PowerShell-lel Windows 10 ügyféltanúsítványok előállításához.</span><span class="sxs-lookup"><span data-stu-id="664ff-105">This article shows you how to create a self-signed root certificate and generate client certificates using PowerShell on Windows 10.</span></span> <span data-ttu-id="664ff-106">Ha a keresett pont-hely konfigurációs lépések, például a legfelső szintű tanúsítványok feltöltéséről válassza az "konfigurálása pont-pont" cikkekben az alábbi listából:</span><span class="sxs-lookup"><span data-stu-id="664ff-106">If you are looking for Point-to-Site configuration steps, such as how to upload root certificates, select one of the 'Configure Point-to-Site' articles from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="664ff-107">Hozzon létre önaláírt tanúsítványokat - PowerShell</span><span class="sxs-lookup"><span data-stu-id="664ff-107">Create self-signed certificates - PowerShell</span></span>](vpn-gateway-certificates-point-to-site.md)
> * [<span data-ttu-id="664ff-108">Hozzon létre önaláírt tanúsítványokat - MakeCert</span><span class="sxs-lookup"><span data-stu-id="664ff-108">Create self-signed certificates - MakeCert</span></span>](vpn-gateway-certificates-point-to-site-makecert.md)
> * [<span data-ttu-id="664ff-109">Pont – hely - Resource Manager - Azure-portál konfigurálása</span><span class="sxs-lookup"><span data-stu-id="664ff-109">Configure Point-to-Site - Resource Manager - Azure portal</span></span>](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="664ff-110">Pont – hely - erőforrás-kezelő - PowerShell konfigurálása</span><span class="sxs-lookup"><span data-stu-id="664ff-110">Configure Point-to-Site - Resource Manager - PowerShell</span></span>](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [<span data-ttu-id="664ff-111">Pont – hely - klasszikus - Azure-portál konfigurálása</span><span class="sxs-lookup"><span data-stu-id="664ff-111">Configure Point-to-Site - Classic - Azure portal</span></span>](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 


<span data-ttu-id="664ff-112">Ez a cikk a Windows 10 rendszerű számítógépre kell hajtsa végre a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="664ff-112">You must perform the steps in this article on a computer running Windows 10.</span></span> <span data-ttu-id="664ff-113">A PowerShell-parancsmagok, amelyekkel lehet tanúsítványokat létrehozni a Windows 10 operációs rendszer része, és a Windows más verziói nem működnek.</span><span class="sxs-lookup"><span data-stu-id="664ff-113">The PowerShell cmdlets that you use to generate certificates are part of the Windows 10 operating system and do not work on other versions of Windows.</span></span> <span data-ttu-id="664ff-114">A Windows 10 rendszerű számítógépeket csak akkor tanúsítványainak előállításához szükséges.</span><span class="sxs-lookup"><span data-stu-id="664ff-114">The Windows 10 computer is only needed to generate the certificates.</span></span> <span data-ttu-id="664ff-115">Ha a tanúsítványok jönnek létre, feltöltheti ezeket, vagy telepítse őket a támogatott ügyfél operációs rendszereken.</span><span class="sxs-lookup"><span data-stu-id="664ff-115">Once the certificates are generated, you can upload them, or install them on any supported client operating system.</span></span> 

<span data-ttu-id="664ff-116">Ha nincs hozzáférése a Windows 10 rendszerű számítógépeket, [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) tanúsítványainak létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="664ff-116">If you do not have access to a Windows 10 computer, you can use [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) to generate certificates.</span></span> <span data-ttu-id="664ff-117">A tanúsítványok, létrehozhat módszerek használatával is telepíthető [támogatott](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) ügyfél operációs rendszerét.</span><span class="sxs-lookup"><span data-stu-id="664ff-117">The certificates that you generate using either method can be installed on any [supported](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) client operating system.</span></span>

## <span data-ttu-id="664ff-118"><a name="rootcert"></a>Hozzon létre egy önaláírt legfelső szintű tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="664ff-118"><a name="rootcert"></a>Create a self-signed root certificate</span></span>

<span data-ttu-id="664ff-119">A New-SelfSignedCertificate parancsmag segítségével hozzon létre egy önaláírt legfelső szintű tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="664ff-119">Use the New-SelfSignedCertificate cmdlet to create a self-signed root certificate.</span></span> <span data-ttu-id="664ff-120">További információkért lásd: [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="664ff-120">For additional parameter information, see [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span></span>

1. <span data-ttu-id="664ff-121">Windows 10 rendszerű számítógépeken nyisson meg egy Windows PowerShell-konzolt emelt szintű jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="664ff-121">From a computer running Windows 10, open a Windows PowerShell console with elevated privileges.</span></span>
2. <span data-ttu-id="664ff-122">Az alábbi példát követve létrehozni az önaláírt legfelső szintű tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="664ff-122">Use the following example to create the self-signed root certificate.</span></span> <span data-ttu-id="664ff-123">Az alábbi példa létrehoz egy önaláírt legfelső szintű tanúsítványt, "P2SRootCert", amely automatikusan telepíti a "Tanúsítványok-aktuális User\Personal\Certificates" nevű.</span><span class="sxs-lookup"><span data-stu-id="664ff-123">The following example creates a self-signed root certificate named 'P2SRootCert' that is automatically installed in 'Certificates-Current User\Personal\Certificates'.</span></span> <span data-ttu-id="664ff-124">A tanúsítvány megtekintéséhez nyissa meg *certmgr.msc*, vagy *kezelheti a felhasználói tanúsítványok*.</span><span class="sxs-lookup"><span data-stu-id="664ff-124">You can view the certificate by opening *certmgr.msc*, or *Manage User Certificates*.</span></span>

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

### <span data-ttu-id="664ff-125"><a name="cer"></a>Exportálja a nyilvános kulcsot (.cer)</span><span class="sxs-lookup"><span data-stu-id="664ff-125"><a name="cer"></a>Export the public key (.cer)</span></span>

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

<span data-ttu-id="664ff-126">A exported.cer fájlt fel kell tölteni, az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="664ff-126">The exported.cer file must be uploaded to Azure.</span></span> <span data-ttu-id="664ff-127">Útmutatásért lásd: [egy pont – hely kapcsolat beállítása](vpn-gateway-howto-point-to-site-rm-ps.md#upload).</span><span class="sxs-lookup"><span data-stu-id="664ff-127">For instructions, see [Configure a Point-to-Site connection](vpn-gateway-howto-point-to-site-rm-ps.md#upload).</span></span> <span data-ttu-id="664ff-128">Egy további megbízható legfelső szintű tanúsítvány hozzáadása [ebben a szakaszban](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) a cikk.</span><span class="sxs-lookup"><span data-stu-id="664ff-128">To add an additional trusted root certificate, [this section](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) of the article.</span></span>

### <a name="export-the-self-signed-root-certificate-and-public-key-to-store-it-optional"></a><span data-ttu-id="664ff-129">Exportálhatja az önaláírt legfelső szintű tanúsítvány és nyilvános kulcs tárolja (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="664ff-129">Export the self-signed root certificate and public key to store it (optional)</span></span>

<span data-ttu-id="664ff-130">Érdemes lehet önaláírt legfelső szintű tanúsítványának exportálása és tárolja biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="664ff-130">You may want to export the self-signed root certificate and store it safely.</span></span> <span data-ttu-id="664ff-131">Ha kell, később egy másik számítógépre telepítse, és további ügyféltanúsítványok előállításához vagy egy másik .cer-fájl exportálását.</span><span class="sxs-lookup"><span data-stu-id="664ff-131">If need be, you can later install it on another computer and generate more client certificates, or export another .cer file.</span></span> <span data-ttu-id="664ff-132">Az önaláírt legfelső szintű tanúsítvány exportálása a .pfx fájlhoz, válassza ki a legfelső szintű tanúsítványt, és használja ugyanazokat a lépéseket, a [ügyféltanúsítvány exportálása](#clientexport).</span><span class="sxs-lookup"><span data-stu-id="664ff-132">To export the self-signed root certificate as a .pfx, select the root certificate and use the same steps as described in [Export a client certificate](#clientexport).</span></span>

## <span data-ttu-id="664ff-133"><a name="clientcert"></a>Ügyfél-tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="664ff-133"><a name="clientcert"></a>Generate a client certificate</span></span>

<span data-ttu-id="664ff-134">Minden, a virtuális hálózathoz pont–hely kapcsolattal csatlakozó ügyfélszámítógépnek rendelkeznie kell telepített ügyféltanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="664ff-134">Each client computer that connects to a VNet using Point-to-Site must have a client certificate installed.</span></span> <span data-ttu-id="664ff-135">Hozzon létre egy ügyféltanúsítványt a önaláírt legfelső szintű tanúsítvány és exportálni, és telepíti az ügyféltanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="664ff-135">You generate a client certificate from the self-signed root certificate, and then export and install the client certificate.</span></span> <span data-ttu-id="664ff-136">Ha az ügyféltanúsítvány nincs telepítve, a hitelesítés meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="664ff-136">If the client certificate is not installed, authentication fails.</span></span> 

<span data-ttu-id="664ff-137">A következő lépések végigvezetik történő tanúsítványgenerálás során egy ügyfél egy önaláírt legfelső szintű tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="664ff-137">The following steps walk you through generating a client certificate from a self-signed root certificate.</span></span> <span data-ttu-id="664ff-138">Előfordulhat, hogy több ügyféltanúsítványt generálása ugyanazon főtanúsítványhoz.</span><span class="sxs-lookup"><span data-stu-id="664ff-138">You may generate multiple client certificates from the same root certificate.</span></span> <span data-ttu-id="664ff-139">Az alábbi lépéseket követve ügyféltanúsítványok létrehozásakor az ügyféltanúsítvány automatikusan települ azon a számítógépen, amelyet a tanúsítvány létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="664ff-139">When you generate client certificates using the steps below, the client certificate is automatically installed on the computer that you used to generate the certificate.</span></span> <span data-ttu-id="664ff-140">Ha egy ügyfél-tanúsítványt egy másik ügyfélszámítógépen telepíteni szeretné, exportálhatja a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="664ff-140">If you want to install a client certificate on another client computer, you can export the certificate.</span></span>

<span data-ttu-id="664ff-141">A példák a New-SelfSignedCertificate parancsmaggal hozza létre az ügyfél, amely egy év lejár.</span><span class="sxs-lookup"><span data-stu-id="664ff-141">The examples use the New-SelfSignedCertificate cmdlet to generate a client certificate that expires in one year.</span></span> <span data-ttu-id="664ff-142">További információkat, például a különböző lejárati érték megadásakor az ügyféltanúsítványt, lásd: [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="664ff-142">For additional parameter information, such as setting a different expiration value for the client certificate, see [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span></span>

### <a name="example-1"></a><span data-ttu-id="664ff-143">1. példa</span><span class="sxs-lookup"><span data-stu-id="664ff-143">Example 1</span></span>

<span data-ttu-id="664ff-144">A példa a deklarált "$cert" változót előző szakaszából.</span><span class="sxs-lookup"><span data-stu-id="664ff-144">This example uses the declared '$cert' variable from the previous section.</span></span> <span data-ttu-id="664ff-145">Ha a gyökér önaláírt tanúsítvány létrehozása után a PowerShell-konzolon zárva, vagy egy új PowerShell-konzol munkamenetet további ügyfél tanúsítványok készíti, lépésekkel [2. példa](#ex2).</span><span class="sxs-lookup"><span data-stu-id="664ff-145">If you closed the PowerShell console after creating the self-signed root certificate, or are creating additional client certificates in a new PowerShell console session, use the steps in [Example 2](#ex2).</span></span>

<span data-ttu-id="664ff-146">Módosítsa, majd futtassa a példa egy ügyfél-tanúsítvány előállításához.</span><span class="sxs-lookup"><span data-stu-id="664ff-146">Modify and run the example to generate a client certificate.</span></span> <span data-ttu-id="664ff-147">Ha az alábbi példa azt a módosítása nélkül futtatja, eredménye "P2SChildCert" nevű ügyfél-tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="664ff-147">If you run the following example without modifying it, the result is a client certificate named 'P2SChildCert'.</span></span>  <span data-ttu-id="664ff-148">Ha szeretné a gyermek tanúsítvány elnevezése valami mást, módosítsa a CN-érték.</span><span class="sxs-lookup"><span data-stu-id="664ff-148">If you want to name the child certificate something else, modify the CN value.</span></span> <span data-ttu-id="664ff-149">Ne módosítsa a TextExtension ebben a példában futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="664ff-149">Do not change the TextExtension when running this example.</span></span> <span data-ttu-id="664ff-150">Az ügyfél tanúsítványát, létrehozhat "Tanúsítványok - aktuális User\Personal\Certificates" automatikusan telepíti a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="664ff-150">The client certificate that you generate is automatically installed in 'Certificates - Current User\Personal\Certificates' on your computer.</span></span>

```powershell
New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <span data-ttu-id="664ff-151"><a name="ex2"></a>2. példa</span><span class="sxs-lookup"><span data-stu-id="664ff-151"><a name="ex2"></a>Example 2</span></span>

<span data-ttu-id="664ff-152">Ha további ügyféltanúsítványok létrehozni, vagy nem használja az ugyanazon a önaláírt legfelső szintű tanúsítvány létrehozásához használt PowerShell-munkamenetet, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="664ff-152">If you are creating additional client certificates, or are not using the same PowerShell session that you used to create your self-signed root certificate, use the following steps:</span></span>

1. <span data-ttu-id="664ff-153">Azonosítsa a önaláírt legfelső szintű tanúsítvány, amely telepítve van a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="664ff-153">Identify the self-signed root certificate that is installed on the computer.</span></span> <span data-ttu-id="664ff-154">Ez a parancsmag a számítógépen telepített tanúsítványok listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="664ff-154">This cmdlet returns a list of certificates that are installed on your computer.</span></span>

  ```powershell
  Get-ChildItem -Path “Cert:\CurrentUser\My”
  ```
2. <span data-ttu-id="664ff-155">Keresse meg a tulajdonos nevét, a visszaadott listából, majd másolja le az ujjlenyomatot, amely egy szövegfájlba mellette található.</span><span class="sxs-lookup"><span data-stu-id="664ff-155">Locate the subject name from the returned list, then copy the thumbprint that is located next to it to a text file.</span></span> <span data-ttu-id="664ff-156">A következő példában két tanúsítványokat is.</span><span class="sxs-lookup"><span data-stu-id="664ff-156">In the following example, there are two certificates.</span></span> <span data-ttu-id="664ff-157">A CN-név esetén a önaláírt legfelső szintű tanúsítvány, amelyből gyermek tanúsítványt létrehozni kívánja.</span><span class="sxs-lookup"><span data-stu-id="664ff-157">The CN name is the name of the self-signed root certificate from which you want to generate a child certificate.</span></span> <span data-ttu-id="664ff-158">Ebben az esetben "P2SRootCert".</span><span class="sxs-lookup"><span data-stu-id="664ff-158">In this case, 'P2SRootCert'.</span></span>

  ```
  Thumbprint                                Subject
  
  AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
  7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
  ```
3. <span data-ttu-id="664ff-159">A legfelső szintű tanúsítvány ujjlenyomata az előző lépésben segítségével változó deklarálható.</span><span class="sxs-lookup"><span data-stu-id="664ff-159">Declare a variable for the root certificate using the thumbprint from the previous step.</span></span> <span data-ttu-id="664ff-160">Cserélje le, amelyből el kívánja létrehozni a tanúsítványt a főtanúsítvány ujjlenyomatának UJJLENYOMATA.</span><span class="sxs-lookup"><span data-stu-id="664ff-160">Replace THUMBPRINT with the thumbprint of the root certificate from which you want to generate a child certificate.</span></span>

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
  ```

  <span data-ttu-id="664ff-161">Például az ujjlenyomat P2SRootCert használja az előző lépésben, a változó néz ki:</span><span class="sxs-lookup"><span data-stu-id="664ff-161">For example, using the thumbprint for P2SRootCert in the previous step, the variable looks like this:</span></span>

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
  ```
4.  <span data-ttu-id="664ff-162">Módosítsa, majd futtassa a példa egy ügyfél-tanúsítvány előállításához.</span><span class="sxs-lookup"><span data-stu-id="664ff-162">Modify and run the example to generate a client certificate.</span></span> <span data-ttu-id="664ff-163">Ha az alábbi példa azt a módosítása nélkül futtatja, eredménye "P2SChildCert" nevű ügyfél-tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="664ff-163">If you run the following example without modifying it, the result is a client certificate named 'P2SChildCert'.</span></span> <span data-ttu-id="664ff-164">Ha szeretné a gyermek tanúsítvány elnevezése valami mást, módosítsa a CN-érték.</span><span class="sxs-lookup"><span data-stu-id="664ff-164">If you want to name the child certificate something else, modify the CN value.</span></span> <span data-ttu-id="664ff-165">Ne módosítsa a TextExtension ebben a példában futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="664ff-165">Do not change the TextExtension when running this example.</span></span> <span data-ttu-id="664ff-166">Az ügyfél tanúsítványát, létrehozhat "Tanúsítványok - aktuális User\Personal\Certificates" automatikusan telepíti a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="664ff-166">The client certificate that you generate is automatically installed in 'Certificates - Current User\Personal\Certificates' on your computer.</span></span>

  ```powershell
  New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <span data-ttu-id="664ff-167"><a name="clientexport"></a>Ügyfél-tanúsítvány exportálása</span><span class="sxs-lookup"><span data-stu-id="664ff-167"><a name="clientexport"></a>Export a client certificate</span></span>   

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

## <span data-ttu-id="664ff-168"><a name="install"></a>Az exportált ügyféltanúsítvány telepítése</span><span class="sxs-lookup"><span data-stu-id="664ff-168"><a name="install"></a>Install an exported client certificate</span></span>

[!INCLUDE [Install client certificate](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="next-steps"></a><span data-ttu-id="664ff-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="664ff-169">Next steps</span></span>

<span data-ttu-id="664ff-170">A pont-hely konfigurációs folytatásához.</span><span class="sxs-lookup"><span data-stu-id="664ff-170">Continue with your Point-to-Site configuration.</span></span> 

* <span data-ttu-id="664ff-171">A **erőforrás-kezelő** telepítési modell lépéseket lásd: [egy Vnetet egy pont – hely kapcsolat beállítása](vpn-gateway-howto-point-to-site-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="664ff-171">For **Resource Manager** deployment model steps, see [Configure a Point-to-Site connection to a VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md).</span></span> 
* <span data-ttu-id="664ff-172">A **klasszikus** telepítési modell lépéseket lásd: [egy virtuális hálózat (klasszikus) pont – hely típusú VPN-kapcsolat konfigurálva](vpn-gateway-howto-point-to-site-classic-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="664ff-172">For **classic** deployment model steps, see [Configure a Point-to-Site VPN connection to a VNet (classic)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).</span></span>