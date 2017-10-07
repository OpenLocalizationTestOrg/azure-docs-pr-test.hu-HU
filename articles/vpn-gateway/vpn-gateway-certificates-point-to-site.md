---
title: "Készítése és a pont-pont tanúsítványok exportálása: PowerShell: Azure |} Microsoft Docs"
description: "A cikkben lépéseket toocreate önaláírt legfelső szintű tanúsítvány hello nyilvános kulcsának exportálásához és a PowerShell-lel Windows 10 ügyféltanúsítványok előállításához."
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
ms.openlocfilehash: 11dda015368cda5ce9799fcc4f01d7c542b84fe8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-powershell-on-windows-10"></a><span data-ttu-id="de63a-103">Létrehozása és exportálása a tanúsítványok a PowerShell-lel Windows 10 pont – hely kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="de63a-103">Generate and export certificates for Point-to-Site connections using PowerShell on Windows 10</span></span>

<span data-ttu-id="de63a-104">Pont – hely kapcsolatok tanúsítványok tooauthenticate használja.</span><span class="sxs-lookup"><span data-stu-id="de63a-104">Point-to-Site connections use certificates tooauthenticate.</span></span> <span data-ttu-id="de63a-105">Ez a cikk azt ismerteti, hogyan toocreate önaláírt a legfelső szintű tanúsítványt, és a PowerShell-lel Windows 10 ügyféltanúsítványok előállításához.</span><span class="sxs-lookup"><span data-stu-id="de63a-105">This article shows you how toocreate a self-signed root certificate and generate client certificates using PowerShell on Windows 10.</span></span> <span data-ttu-id="de63a-106">Ha a pont-hely konfigurációs lépések, például hogyan tooupload legfelső szintű tanúsítványok, a következő hello hello "konfigurálása pont-pont" cikkek válasszon ki egy listából:</span><span class="sxs-lookup"><span data-stu-id="de63a-106">If you are looking for Point-to-Site configuration steps, such as how tooupload root certificates, select one of hello 'Configure Point-to-Site' articles from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="de63a-107">Hozzon létre önaláírt tanúsítványokat - PowerShell</span><span class="sxs-lookup"><span data-stu-id="de63a-107">Create self-signed certificates - PowerShell</span></span>](vpn-gateway-certificates-point-to-site.md)
> * [<span data-ttu-id="de63a-108">Hozzon létre önaláírt tanúsítványokat - MakeCert</span><span class="sxs-lookup"><span data-stu-id="de63a-108">Create self-signed certificates - MakeCert</span></span>](vpn-gateway-certificates-point-to-site-makecert.md)
> * [<span data-ttu-id="de63a-109">Pont – hely - Resource Manager - Azure-portál konfigurálása</span><span class="sxs-lookup"><span data-stu-id="de63a-109">Configure Point-to-Site - Resource Manager - Azure portal</span></span>](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="de63a-110">Pont – hely - erőforrás-kezelő - PowerShell konfigurálása</span><span class="sxs-lookup"><span data-stu-id="de63a-110">Configure Point-to-Site - Resource Manager - PowerShell</span></span>](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [<span data-ttu-id="de63a-111">Pont – hely - klasszikus - Azure-portál konfigurálása</span><span class="sxs-lookup"><span data-stu-id="de63a-111">Configure Point-to-Site - Classic - Azure portal</span></span>](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 


<span data-ttu-id="de63a-112">Ebben a cikkben a Windows 10 rendszerű számítógépre hello lépéseket kell végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="de63a-112">You must perform hello steps in this article on a computer running Windows 10.</span></span> <span data-ttu-id="de63a-113">hello PowerShell-parancsmagok használata toogenerate tanúsítványok hello Windows 10 operációs rendszer része, és a Windows más verziói nem működnek.</span><span class="sxs-lookup"><span data-stu-id="de63a-113">hello PowerShell cmdlets that you use toogenerate certificates are part of hello Windows 10 operating system and do not work on other versions of Windows.</span></span> <span data-ttu-id="de63a-114">Windows 10 hello számítógép csak a szükséges toogenerate hello tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="de63a-114">hello Windows 10 computer is only needed toogenerate hello certificates.</span></span> <span data-ttu-id="de63a-115">Hello tanúsítványok jönnek létre, ha feltöltheti ezeket, vagy telepítse őket a támogatott ügyfél operációs rendszereken.</span><span class="sxs-lookup"><span data-stu-id="de63a-115">Once hello certificates are generated, you can upload them, or install them on any supported client operating system.</span></span> 

<span data-ttu-id="de63a-116">Ha Ön nem rendelkezik hozzáféréssel a Windows 10 tooa számítógép, [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) toogenerate tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="de63a-116">If you do not have access tooa Windows 10 computer, you can use [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) toogenerate certificates.</span></span> <span data-ttu-id="de63a-117">hello tanúsítványok módszerek használatával generáló telepíthetők bármely [támogatott](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) ügyfél operációs rendszerét.</span><span class="sxs-lookup"><span data-stu-id="de63a-117">hello certificates that you generate using either method can be installed on any [supported](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) client operating system.</span></span>

## <span data-ttu-id="de63a-118"><a name="rootcert"></a>Hozzon létre egy önaláírt legfelső szintű tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="de63a-118"><a name="rootcert"></a>Create a self-signed root certificate</span></span>

<span data-ttu-id="de63a-119">Hello New-SelfSignedCertificate parancsmag toocreate önaláírt legfelső szintű tanúsítvány használatára.</span><span class="sxs-lookup"><span data-stu-id="de63a-119">Use hello New-SelfSignedCertificate cmdlet toocreate a self-signed root certificate.</span></span> <span data-ttu-id="de63a-120">További információkért lásd: [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="de63a-120">For additional parameter information, see [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span></span>

1. <span data-ttu-id="de63a-121">Windows 10 rendszerű számítógépeken nyisson meg egy Windows PowerShell-konzolt emelt szintű jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="de63a-121">From a computer running Windows 10, open a Windows PowerShell console with elevated privileges.</span></span>
2. <span data-ttu-id="de63a-122">A következő példa toocreate hello önaláírt legfelső szintű tanúsítvány hello használata.</span><span class="sxs-lookup"><span data-stu-id="de63a-122">Use hello following example toocreate hello self-signed root certificate.</span></span> <span data-ttu-id="de63a-123">hello alábbi példa létrehoz egy önaláírt legfelső szintű tanúsítványt "P2SRootCert", amely automatikusan telepíti a "Tanúsítványok-aktuális User\Personal\Certificates" nevű.</span><span class="sxs-lookup"><span data-stu-id="de63a-123">hello following example creates a self-signed root certificate named 'P2SRootCert' that is automatically installed in 'Certificates-Current User\Personal\Certificates'.</span></span> <span data-ttu-id="de63a-124">Hello tanúsítvány megtekintéséhez nyissa meg *certmgr.msc*, vagy *kezelheti a felhasználói tanúsítványok*.</span><span class="sxs-lookup"><span data-stu-id="de63a-124">You can view hello certificate by opening *certmgr.msc*, or *Manage User Certificates*.</span></span>

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

### <span data-ttu-id="de63a-125"><a name="cer"></a>Exportálás hello nyilvános kulcsát (.cer)</span><span class="sxs-lookup"><span data-stu-id="de63a-125"><a name="cer"></a>Export hello public key (.cer)</span></span>

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

<span data-ttu-id="de63a-126">hello exported.cer fájl feltöltött tooAzure kell lennie.</span><span class="sxs-lookup"><span data-stu-id="de63a-126">hello exported.cer file must be uploaded tooAzure.</span></span> <span data-ttu-id="de63a-127">Útmutatásért lásd: [egy pont – hely kapcsolat beállítása](vpn-gateway-howto-point-to-site-rm-ps.md#upload).</span><span class="sxs-lookup"><span data-stu-id="de63a-127">For instructions, see [Configure a Point-to-Site connection](vpn-gateway-howto-point-to-site-rm-ps.md#upload).</span></span> <span data-ttu-id="de63a-128">egy további megbízható legfelső szintű tanúsítvány tooadd [ebben a szakaszban](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) hello cikk.</span><span class="sxs-lookup"><span data-stu-id="de63a-128">tooadd an additional trusted root certificate, [this section](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) of hello article.</span></span>

### <a name="export-hello-self-signed-root-certificate-and-public-key-toostore-it-optional"></a><span data-ttu-id="de63a-129">Hello önaláírt legfelső szintű tanúsítvány és nyilvános kulcs toostore exportálja azt (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="de63a-129">Export hello self-signed root certificate and public key toostore it (optional)</span></span>

<span data-ttu-id="de63a-130">Érdemes lehet tooexport hello önaláírt a legfelső szintű tanúsítványt, és tárolja biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="de63a-130">You may want tooexport hello self-signed root certificate and store it safely.</span></span> <span data-ttu-id="de63a-131">Ha kell, később egy másik számítógépre telepítse, és további ügyféltanúsítványok előállításához vagy egy másik .cer-fájl exportálását.</span><span class="sxs-lookup"><span data-stu-id="de63a-131">If need be, you can later install it on another computer and generate more client certificates, or export another .cer file.</span></span> <span data-ttu-id="de63a-132">tooexport hello önaláírt legfelső szintű tanúsítvány, egy .pfx, jelölje be hello legfelső szintű tanúsítvány és használata hello ugyanazokat a lépéseket a [ügyféltanúsítvány exportálása](#clientexport).</span><span class="sxs-lookup"><span data-stu-id="de63a-132">tooexport hello self-signed root certificate as a .pfx, select hello root certificate and use hello same steps as described in [Export a client certificate](#clientexport).</span></span>

## <span data-ttu-id="de63a-133"><a name="clientcert"></a>Ügyfél-tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="de63a-133"><a name="clientcert"></a>Generate a client certificate</span></span>

<span data-ttu-id="de63a-134">Minden ügyfélszámítógép, amely a tooa VNet pont-pont használatával telepített ügyféltanúsítvánnyal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="de63a-134">Each client computer that connects tooa VNet using Point-to-Site must have a client certificate installed.</span></span> <span data-ttu-id="de63a-135">Hozzon létre egy ügyféltanúsítványt hello önaláírt legfelső szintű tanúsítványt, és exportálni, és hello telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="de63a-135">You generate a client certificate from hello self-signed root certificate, and then export and install hello client certificate.</span></span> <span data-ttu-id="de63a-136">Ha hello ügyféltanúsítvány nincs telepítve, a hitelesítés meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="de63a-136">If hello client certificate is not installed, authentication fails.</span></span> 

<span data-ttu-id="de63a-137">hello következő lépések végigvezetik történő tanúsítványgenerálás során egy ügyfél egy önaláírt legfelső szintű tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="de63a-137">hello following steps walk you through generating a client certificate from a self-signed root certificate.</span></span> <span data-ttu-id="de63a-138">Előfordulhat, hogy több ügyféltanúsítványt generálása hello ugyanazon főtanúsítványhoz.</span><span class="sxs-lookup"><span data-stu-id="de63a-138">You may generate multiple client certificates from hello same root certificate.</span></span> <span data-ttu-id="de63a-139">Ügyféltanúsítványok alábbi hello lépéseket használhatja generálásakor hello ügyféltanúsítvány automatikusan számítógépen hello toogenerate hello tanúsítvány használatát.</span><span class="sxs-lookup"><span data-stu-id="de63a-139">When you generate client certificates using hello steps below, hello client certificate is automatically installed on hello computer that you used toogenerate hello certificate.</span></span> <span data-ttu-id="de63a-140">Ha azt szeretné, hogy egy másik ügyfélszámítógépen ügyféltanúsítványt tooinstall, exportálhatja hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="de63a-140">If you want tooinstall a client certificate on another client computer, you can export hello certificate.</span></span>

<span data-ttu-id="de63a-141">a példákban hello hello New-SelfSignedCertificate parancsmag toogenerate egy ügyféltanúsítványt, amely egy év lejár.</span><span class="sxs-lookup"><span data-stu-id="de63a-141">hello examples use hello New-SelfSignedCertificate cmdlet toogenerate a client certificate that expires in one year.</span></span> <span data-ttu-id="de63a-142">További információkat, például a különböző lejárati érték hello ügyféltanúsítványt, lásd: [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="de63a-142">For additional parameter information, such as setting a different expiration value for hello client certificate, see [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span></span>

### <a name="example-1"></a><span data-ttu-id="de63a-143">1. példa</span><span class="sxs-lookup"><span data-stu-id="de63a-143">Example 1</span></span>

<span data-ttu-id="de63a-144">A példa az előző szakasz hello "$cert" változó deklarált hello.</span><span class="sxs-lookup"><span data-stu-id="de63a-144">This example uses hello declared '$cert' variable from hello previous section.</span></span> <span data-ttu-id="de63a-145">Ha korábban bezárta hello PowerShell konzol létrehozása önaláírt legfelső szintű tanúsítvány hello, vagy egy új PowerShell-konzol munkamenetet további ügyfél tanúsítványok készíti után, hajtsa végre a hello lépéseket [2. példa](#ex2).</span><span class="sxs-lookup"><span data-stu-id="de63a-145">If you closed hello PowerShell console after creating hello self-signed root certificate, or are creating additional client certificates in a new PowerShell console session, use hello steps in [Example 2](#ex2).</span></span>

<span data-ttu-id="de63a-146">Módosíthatja, és futtassa a hello példa toogenerate ügyféltanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="de63a-146">Modify and run hello example toogenerate a client certificate.</span></span> <span data-ttu-id="de63a-147">Ha az alábbi példa azt a módosítása nélkül hello futtatja, hello eredménye "P2SChildCert" nevű ügyfél-tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="de63a-147">If you run hello following example without modifying it, hello result is a client certificate named 'P2SChildCert'.</span></span>  <span data-ttu-id="de63a-148">Ha azt szeretné, hogy tooname hello a tanúsítványt valami mást, módosítsa a hello CN-értéket.</span><span class="sxs-lookup"><span data-stu-id="de63a-148">If you want tooname hello child certificate something else, modify hello CN value.</span></span> <span data-ttu-id="de63a-149">Ne változtassa hello TextExtension ebben a példában futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="de63a-149">Do not change hello TextExtension when running this example.</span></span> <span data-ttu-id="de63a-150">Létrehozhat hello ügyféltanúsítvány "Tanúsítványok - aktuális User\Personal\Certificates" automatikusan települ a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="de63a-150">hello client certificate that you generate is automatically installed in 'Certificates - Current User\Personal\Certificates' on your computer.</span></span>

```powershell
New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <span data-ttu-id="de63a-151"><a name="ex2"></a>2. példa</span><span class="sxs-lookup"><span data-stu-id="de63a-151"><a name="ex2"></a>Example 2</span></span>

<span data-ttu-id="de63a-152">Ha Ön által létrehozott további ügyfél tanúsítványokat, és vannak nem használ hello azonos PowerShell-munkamenetet, amellyel toocreate a önaláírt főtanúsítványt, használja hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="de63a-152">If you are creating additional client certificates, or are not using hello same PowerShell session that you used toocreate your self-signed root certificate, use hello following steps:</span></span>

1. <span data-ttu-id="de63a-153">Azonosítsa a hello önaláírt főtanúsítványt hello számítógépen telepítve van.</span><span class="sxs-lookup"><span data-stu-id="de63a-153">Identify hello self-signed root certificate that is installed on hello computer.</span></span> <span data-ttu-id="de63a-154">Ez a parancsmag a számítógépen telepített tanúsítványok listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="de63a-154">This cmdlet returns a list of certificates that are installed on your computer.</span></span>

  ```powershell
  Get-ChildItem -Path “Cert:\CurrentUser\My”
  ```
2. <span data-ttu-id="de63a-155">Keresse meg a hello tulajdonos nevét a listában, majd a Másolás hello ujjlenyomat, amely található következő tooit tooa szöveg visszaadott hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="de63a-155">Locate hello subject name from hello returned list, then copy hello thumbprint that is located next tooit tooa text file.</span></span> <span data-ttu-id="de63a-156">A következő példa hello két tanúsítványokat is.</span><span class="sxs-lookup"><span data-stu-id="de63a-156">In hello following example, there are two certificates.</span></span> <span data-ttu-id="de63a-157">hello CN-név, amelyből el kívánja toogenerate gyermek tanúsítvány hello önaláírt legfelső szintű tanúsítvány hello neve.</span><span class="sxs-lookup"><span data-stu-id="de63a-157">hello CN name is hello name of hello self-signed root certificate from which you want toogenerate a child certificate.</span></span> <span data-ttu-id="de63a-158">Ebben az esetben "P2SRootCert".</span><span class="sxs-lookup"><span data-stu-id="de63a-158">In this case, 'P2SRootCert'.</span></span>

  ```
  Thumbprint                                Subject
  
  AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
  7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
  ```
3. <span data-ttu-id="de63a-159">Hello legfelső szintű tanúsítvány hello ujjlenyomata hello előző lépésben változó deklarálható.</span><span class="sxs-lookup"><span data-stu-id="de63a-159">Declare a variable for hello root certificate using hello thumbprint from hello previous step.</span></span> <span data-ttu-id="de63a-160">Cserélje le UJJLENYOMATÁT hello hello főtanúsítványt, amelyből el kívánja toogenerate gyermek tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="de63a-160">Replace THUMBPRINT with hello thumbprint of hello root certificate from which you want toogenerate a child certificate.</span></span>

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
  ```

  <span data-ttu-id="de63a-161">Például P2SRootCert hello előző lépésben hello ujjlenyomat alkalmaz, hello változó néz ki:</span><span class="sxs-lookup"><span data-stu-id="de63a-161">For example, using hello thumbprint for P2SRootCert in hello previous step, hello variable looks like this:</span></span>

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
  ```
4.  <span data-ttu-id="de63a-162">Módosíthatja, és futtassa a hello példa toogenerate ügyféltanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="de63a-162">Modify and run hello example toogenerate a client certificate.</span></span> <span data-ttu-id="de63a-163">Ha az alábbi példa azt a módosítása nélkül hello futtatja, hello eredménye "P2SChildCert" nevű ügyfél-tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="de63a-163">If you run hello following example without modifying it, hello result is a client certificate named 'P2SChildCert'.</span></span> <span data-ttu-id="de63a-164">Ha azt szeretné, hogy tooname hello a tanúsítványt valami mást, módosítsa a hello CN-értéket.</span><span class="sxs-lookup"><span data-stu-id="de63a-164">If you want tooname hello child certificate something else, modify hello CN value.</span></span> <span data-ttu-id="de63a-165">Ne változtassa hello TextExtension ebben a példában futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="de63a-165">Do not change hello TextExtension when running this example.</span></span> <span data-ttu-id="de63a-166">Létrehozhat hello ügyféltanúsítvány "Tanúsítványok - aktuális User\Personal\Certificates" automatikusan települ a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="de63a-166">hello client certificate that you generate is automatically installed in 'Certificates - Current User\Personal\Certificates' on your computer.</span></span>

  ```powershell
  New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <span data-ttu-id="de63a-167"><a name="clientexport"></a>Ügyfél-tanúsítvány exportálása</span><span class="sxs-lookup"><span data-stu-id="de63a-167"><a name="clientexport"></a>Export a client certificate</span></span>   

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

## <span data-ttu-id="de63a-168"><a name="install"></a>Az exportált ügyféltanúsítvány telepítése</span><span class="sxs-lookup"><span data-stu-id="de63a-168"><a name="install"></a>Install an exported client certificate</span></span>

[!INCLUDE [Install client certificate](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="next-steps"></a><span data-ttu-id="de63a-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="de63a-169">Next steps</span></span>

<span data-ttu-id="de63a-170">A pont-hely konfigurációs folytatásához.</span><span class="sxs-lookup"><span data-stu-id="de63a-170">Continue with your Point-to-Site configuration.</span></span> 

* <span data-ttu-id="de63a-171">A **erőforrás-kezelő** telepítési modell lépéseket lásd: [konfigurálása egy pont – hely kapcsolat tooa VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="de63a-171">For **Resource Manager** deployment model steps, see [Configure a Point-to-Site connection tooa VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md).</span></span> 
* <span data-ttu-id="de63a-172">A **klasszikus** telepítési modell lépéseket lásd: [konfigurálása egy pont – hely típusú VPN-kapcsolat tooa hálózatok (klasszikus)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="de63a-172">For **classic** deployment model steps, see [Configure a Point-to-Site VPN connection tooa VNet (classic)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).</span></span>
