---
title: "aaaSplit egyesítéses biztonsági beállításai |} Microsoft Docs"
description: "Állítson be x409 titkosítási tanúsítványok"
metakeywords: Elastic Database certificates security
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: f9e89c57-61a0-484f-b787-82dae2349cb6
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: 511c04be0598d8a0889aa3e3fcf02be0bf0e96cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="split-merge-security-configuration"></a><span data-ttu-id="a065b-103">Vegyes egyesítéses biztonsági konfiguráció</span><span class="sxs-lookup"><span data-stu-id="a065b-103">Split-merge security configuration</span></span>
<span data-ttu-id="a065b-104">toouse hello vegyes/egyesítés szolgáltatás megfelelően be kell állítani biztonsági.</span><span class="sxs-lookup"><span data-stu-id="a065b-104">toouse hello Split/Merge service, you must correctly configure security.</span></span> <span data-ttu-id="a065b-105">hello szolgáltatást a Microsoft Azure SQL Database hello rugalmas bővítést szolgáltatás részét képezi.</span><span class="sxs-lookup"><span data-stu-id="a065b-105">hello service is part of hello Elastic Scale feature of Microsoft Azure SQL Database.</span></span> <span data-ttu-id="a065b-106">További információkért lásd: [rugalmas méretezési felosztása és egyesítése szolgáltatás oktatóanyag](sql-database-elastic-scale-configure-deploy-split-and-merge.md).</span><span class="sxs-lookup"><span data-stu-id="a065b-106">For more information, see [Elastic Scale Split and Merge Service Tutorial](sql-database-elastic-scale-configure-deploy-split-and-merge.md).</span></span>

## <a name="configuring-certificates"></a><span data-ttu-id="a065b-107">Tanúsítványok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a065b-107">Configuring certificates</span></span>
<span data-ttu-id="a065b-108">Két módon a tanúsítványok konfigurálva legyenek.</span><span class="sxs-lookup"><span data-stu-id="a065b-108">Certificates are configured in two ways.</span></span> 

1. [<span data-ttu-id="a065b-109">tooConfigure hello SSL-tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="a065b-109">tooConfigure hello SSL Certificate</span></span>](#to-configure-the-ssl-certificate)
2. [<span data-ttu-id="a065b-110">Ügyféltanúsítványok tooConfigure</span><span class="sxs-lookup"><span data-stu-id="a065b-110">tooConfigure Client Certificates</span></span>](#to-configure-client-certificates) 

## <a name="tooobtain-certificates"></a><span data-ttu-id="a065b-111">tooobtain tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="a065b-111">tooobtain certificates</span></span>
<span data-ttu-id="a065b-112">Tanúsítványok hello vagy nyilvános hitelesítésszolgáltatótól (CA) szerezhetők [Windows tanúsítványszolgáltatást](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx).</span><span class="sxs-lookup"><span data-stu-id="a065b-112">Certificates can be obtained from public Certificate Authorities (CAs) or from hello [Windows Certificate Service](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx).</span></span> <span data-ttu-id="a065b-113">Ezek azok az előnyben részesített hello módszerek tooobtain tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="a065b-113">These are hello preferred methods tooobtain certificates.</span></span>

<span data-ttu-id="a065b-114">Ha ezek a lehetőségek nem állnak rendelkezésre, létrehozhat **önaláírt tanúsítványokat**.</span><span class="sxs-lookup"><span data-stu-id="a065b-114">If those options are not available, you can generate **self-signed certificates**.</span></span>

## <a name="tools-toogenerate-certificates"></a><span data-ttu-id="a065b-115">Eszközök toogenerate tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="a065b-115">Tools toogenerate certificates</span></span>
* [<span data-ttu-id="a065b-116">MakeCert.exe</span><span class="sxs-lookup"><span data-stu-id="a065b-116">makecert.exe</span></span>](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [<span data-ttu-id="a065b-117">pvk2pfx.exe</span><span class="sxs-lookup"><span data-stu-id="a065b-117">pvk2pfx.exe</span></span>](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="toorun-hello-tools"></a><span data-ttu-id="a065b-118">toorun hello eszközök</span><span class="sxs-lookup"><span data-stu-id="a065b-118">toorun hello tools</span></span>
* <span data-ttu-id="a065b-119">Az a fejlesztő parancssorból Visual Studios, lásd: [Visual Studio parancssorból](http://msdn.microsoft.com/library/ms229859.aspx)</span><span class="sxs-lookup"><span data-stu-id="a065b-119">From a Developer Command Prompt for Visual Studios, see [Visual Studio Command Prompt](http://msdn.microsoft.com/library/ms229859.aspx)</span></span> 
  
    <span data-ttu-id="a065b-120">Ha telepítette, Ugrás:</span><span class="sxs-lookup"><span data-stu-id="a065b-120">If installed, go to:</span></span>
  
        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 
* <span data-ttu-id="a065b-121">Beolvasása a WDK hello [Windows 8.1: Töltse le a készletek és eszközök](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)</span><span class="sxs-lookup"><span data-stu-id="a065b-121">Get hello WDK from [Windows 8.1: Download kits and tools](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)</span></span>

## <a name="tooconfigure-hello-ssl-certificate"></a><span data-ttu-id="a065b-122">tooconfigure hello SSL-tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="a065b-122">tooconfigure hello SSL certificate</span></span>
<span data-ttu-id="a065b-123">Egy SSL-tanúsítvány szükséges tooencrypt hello kommunikációt, és hello kiszolgáló hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a065b-123">A SSL certificate is required tooencrypt hello communication and authenticate hello server.</span></span> <span data-ttu-id="a065b-124">Válassza ki az alábbi három forgatókönyv hello alkalmazható hello, és hajtsa végre az összes lépését:</span><span class="sxs-lookup"><span data-stu-id="a065b-124">Choose hello most applicable of hello three scenarios below, and execute all its steps:</span></span>

### <a name="create-a-new-self-signed-certificate"></a><span data-ttu-id="a065b-125">Hozzon létre egy új önaláírt tanúsítványt</span><span class="sxs-lookup"><span data-stu-id="a065b-125">Create a new self-signed certificate</span></span>
1. [<span data-ttu-id="a065b-126">Önaláírt tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="a065b-126">Create a Self-Signed Certificate</span></span>](#create-a-self-signed-certificate)
2. [<span data-ttu-id="a065b-127">Az önaláírt SSL-tanúsítvány PFX-fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="a065b-127">Create PFX file for Self-Signed SSL Certificate</span></span>](#create-pfx-file-for-self-signed-ssl-certificate)
3. [<span data-ttu-id="a065b-128">Töltse fel az SSL-tanúsítvány tooCloud szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a065b-128">Upload SSL Certificate tooCloud Service</span></span>](#upload-ssl-certificate-to-cloud-service)
4. [<span data-ttu-id="a065b-129">A szolgáltatás konfigurációs fájljában SSL-tanúsítvány frissítése</span><span class="sxs-lookup"><span data-stu-id="a065b-129">Update SSL Certificate in Service Configuration File</span></span>](#update-ssl-certificate-in-service-configuration-file)
5. [<span data-ttu-id="a065b-130">SSL-hitelesítésszolgáltató importálása</span><span class="sxs-lookup"><span data-stu-id="a065b-130">Import SSL Certification Authority</span></span>](#import-ssl-certification-authority)

### <a name="toouse-an-existing-certificate-from-hello-certificate-store"></a><span data-ttu-id="a065b-131">toouse hello tanúsítvány létező tanúsítvány tárolásához</span><span class="sxs-lookup"><span data-stu-id="a065b-131">toouse an existing certificate from hello certificate store</span></span>
1. [<span data-ttu-id="a065b-132">SSL-tanúsítvány exportálása a tanúsítványt a tanúsítványtárolóból</span><span class="sxs-lookup"><span data-stu-id="a065b-132">Export SSL Certificate From Certificate Store</span></span>](#export-ssl-certificate-from-certificate-store)
2. [<span data-ttu-id="a065b-133">Töltse fel az SSL-tanúsítvány tooCloud szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a065b-133">Upload SSL Certificate tooCloud Service</span></span>](#upload-ssl-certificate-to-cloud-service)
3. [<span data-ttu-id="a065b-134">A szolgáltatás konfigurációs fájljában SSL-tanúsítvány frissítése</span><span class="sxs-lookup"><span data-stu-id="a065b-134">Update SSL Certificate in Service Configuration File</span></span>](#update-ssl-certificate-in-service-configuration-file)

### <a name="toouse-an-existing-certificate-in-a-pfx-file"></a><span data-ttu-id="a065b-135">toouse egy meglévő tanúsítványt a PFX-fájl</span><span class="sxs-lookup"><span data-stu-id="a065b-135">toouse an existing certificate in a PFX file</span></span>
1. [<span data-ttu-id="a065b-136">Töltse fel az SSL-tanúsítvány tooCloud szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a065b-136">Upload SSL Certificate tooCloud Service</span></span>](#upload-ssl-certificate-to-cloud-service)
2. [<span data-ttu-id="a065b-137">A szolgáltatás konfigurációs fájljában SSL-tanúsítvány frissítése</span><span class="sxs-lookup"><span data-stu-id="a065b-137">Update SSL Certificate in Service Configuration File</span></span>](#update-ssl-certificate-in-service-configuration-file)

## <a name="tooconfigure-client-certificates"></a><span data-ttu-id="a065b-138">tooconfigure ügyféltanúsítványok</span><span class="sxs-lookup"><span data-stu-id="a065b-138">tooconfigure client certificates</span></span>
<span data-ttu-id="a065b-139">Ügyféltanúsítványok rendelés tooauthenticate kérelmek toohello szolgáltatás kell megadni.</span><span class="sxs-lookup"><span data-stu-id="a065b-139">Client certificates are required in order tooauthenticate requests toohello service.</span></span> <span data-ttu-id="a065b-140">Válassza ki az alábbi három forgatókönyv hello alkalmazható hello, és hajtsa végre az összes lépését:</span><span class="sxs-lookup"><span data-stu-id="a065b-140">Choose hello most applicable of hello three scenarios below, and execute all its steps:</span></span>

### <a name="turn-off-client-certificates"></a><span data-ttu-id="a065b-141">Ügyféltanúsítványok kikapcsolása</span><span class="sxs-lookup"><span data-stu-id="a065b-141">Turn off client certificates</span></span>
1. [<span data-ttu-id="a065b-142">Ügyféltanúsítvány-alapú hitelesítés kikapcsolása</span><span class="sxs-lookup"><span data-stu-id="a065b-142">Turn Off Client Certificate-Based Authentication</span></span>](#turn-off-client-certificate-based-authentication)

### <a name="issue-new-self-signed-client-certificates"></a><span data-ttu-id="a065b-143">Új ügyfél önaláírt tanúsítványokat</span><span class="sxs-lookup"><span data-stu-id="a065b-143">Issue new self-signed client certificates</span></span>
1. [<span data-ttu-id="a065b-144">Hozzon létre egy önaláírt hitelesítésszolgáltató</span><span class="sxs-lookup"><span data-stu-id="a065b-144">Create a Self-Signed Certification Authority</span></span>](#create-a-self-signed-certification-authority)
2. [<span data-ttu-id="a065b-145">Töltse fel a Hitelesítésszolgáltatói tanúsítvány tooCloud szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a065b-145">Upload CA Certificate tooCloud Service</span></span>](#upload-ca-certificate-to-cloud-service)
3. [<span data-ttu-id="a065b-146">A szolgáltatás konfigurációs fájljában Hitelesítésszolgáltatói tanúsítvány frissítése</span><span class="sxs-lookup"><span data-stu-id="a065b-146">Update CA Certificate in Service Configuration File</span></span>](#update-ca-certificate-in-service-configuration-file)
4. [<span data-ttu-id="a065b-147">Ügyfél-tanúsítványok kiállításához</span><span class="sxs-lookup"><span data-stu-id="a065b-147">Issue Client Certificates</span></span>](#issue-client-certificates)
5. [<span data-ttu-id="a065b-148">Az ügyféltanúsítványok PFX-fájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="a065b-148">Create PFX files for Client Certificates</span></span>](#create-pfx-files-for-client-certificates)
6. [<span data-ttu-id="a065b-149">Ügyfél-tanúsítvány importálása</span><span class="sxs-lookup"><span data-stu-id="a065b-149">Import Client Certificate</span></span>](#Import-Client-Certificate)
7. [<span data-ttu-id="a065b-150">Másolja a tanúsítvány-ujjlenyomatok Client</span><span class="sxs-lookup"><span data-stu-id="a065b-150">Copy Client Certificate Thumbprints</span></span>](#copy-client-certificate-thumbprints)
8. [<span data-ttu-id="a065b-151">Konfigurálja az ügyfeleket engedélyezett a hello szolgáltatás konfigurációs fájlja</span><span class="sxs-lookup"><span data-stu-id="a065b-151">Configure Allowed Clients in hello Service Configuration File</span></span>](#configure-allowed-clients-in-the-service-configuration-file)

### <a name="use-existing-client-certificates"></a><span data-ttu-id="a065b-152">Meglévő ügyfél-tanúsítványok használata</span><span class="sxs-lookup"><span data-stu-id="a065b-152">Use existing client certificates</span></span>
1. [<span data-ttu-id="a065b-153">Nyilvános hitelesítésszolgáltató-kulcs megkeresése</span><span class="sxs-lookup"><span data-stu-id="a065b-153">Find CA Public Key</span></span>](#find-ca-public-key)
2. [<span data-ttu-id="a065b-154">Töltse fel a Hitelesítésszolgáltatói tanúsítvány tooCloud szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a065b-154">Upload CA Certificate tooCloud Service</span></span>](#Upload-CA-certificate-to-cloud-service)
3. [<span data-ttu-id="a065b-155">A szolgáltatás konfigurációs fájljában Hitelesítésszolgáltatói tanúsítvány frissítése</span><span class="sxs-lookup"><span data-stu-id="a065b-155">Update CA Certificate in Service Configuration File</span></span>](#Update-CA-Certificate-in-Service-Configuration-File)
4. [<span data-ttu-id="a065b-156">Másolja a tanúsítvány-ujjlenyomatok Client</span><span class="sxs-lookup"><span data-stu-id="a065b-156">Copy Client Certificate Thumbprints</span></span>](#Copy-Client-Certificate-Thumbprints)
5. [<span data-ttu-id="a065b-157">Konfigurálja az ügyfeleket engedélyezett a hello szolgáltatás konfigurációs fájlja</span><span class="sxs-lookup"><span data-stu-id="a065b-157">Configure Allowed Clients in hello Service Configuration File</span></span>](#configure-allowed-clients-in-the-service-configuration-file)
6. [<span data-ttu-id="a065b-158">Konfigurálja az ügyfél visszavonási állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a065b-158">Configure Client Certificate Revocation Check</span></span>](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a><span data-ttu-id="a065b-159">Engedélyezett IP-címek</span><span class="sxs-lookup"><span data-stu-id="a065b-159">Allowed IP addresses</span></span>
<span data-ttu-id="a065b-160">Hozzáférés toohello Szolgáltatásvégpontok IP-címek korlátozott toospecific tartomány lehet.</span><span class="sxs-lookup"><span data-stu-id="a065b-160">Access toohello service endpoints can be restricted toospecific ranges of IP addresses.</span></span>

## <a name="tooconfigure-encryption-for-hello-store"></a><span data-ttu-id="a065b-161">hello Store tooconfigure titkosítás</span><span class="sxs-lookup"><span data-stu-id="a065b-161">tooconfigure encryption for hello store</span></span>
<span data-ttu-id="a065b-162">Tanúsítvány szükség tooencrypt hello hitelesítő adatokat, amelyek hello metaadat-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a065b-162">A certificate is required tooencrypt hello credentials that are stored in hello metadata store.</span></span> <span data-ttu-id="a065b-163">Válassza ki az alábbi három forgatókönyv hello alkalmazható hello, és hajtsa végre az összes lépését:</span><span class="sxs-lookup"><span data-stu-id="a065b-163">Choose hello most applicable of hello three scenarios below, and execute all its steps:</span></span>

### <a name="use-a-new-self-signed-certificate"></a><span data-ttu-id="a065b-164">Egy új önaláírt tanúsítvány használatára</span><span class="sxs-lookup"><span data-stu-id="a065b-164">Use a new self-signed certificate</span></span>
1. [<span data-ttu-id="a065b-165">Önaláírt tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="a065b-165">Create a Self-Signed Certificate</span></span>](#create-a-self-signed-certificate)
2. [<span data-ttu-id="a065b-166">Hozzon létre önaláírt titkosítási tanúsítvány PFX-fájlból</span><span class="sxs-lookup"><span data-stu-id="a065b-166">Create PFX file for Self-Signed Encryption Certificate</span></span>](#create-pfx-file-for-self-signed-ssl-certificate)
3. [<span data-ttu-id="a065b-167">Töltse fel a titkosítási tanúsítvány tooCloud szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a065b-167">Upload Encryption Certificate tooCloud Service</span></span>](#upload-encryption-certificate-to-cloud-service)
4. [<span data-ttu-id="a065b-168">A szolgáltatás konfigurációs fájljában titkosítási tanúsítvány frissítése</span><span class="sxs-lookup"><span data-stu-id="a065b-168">Update Encryption Certificate in Service Configuration File</span></span>](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-from-hello-certificate-store"></a><span data-ttu-id="a065b-169">Létező tanúsítvány használatára hello tanúsítványtárolóból</span><span class="sxs-lookup"><span data-stu-id="a065b-169">Use an existing certificate from hello certificate store</span></span>
1. [<span data-ttu-id="a065b-170">Titkosítási tanúsítvány exportálása a tanúsítványt a tanúsítványtárolóból</span><span class="sxs-lookup"><span data-stu-id="a065b-170">Export Encryption Certificate From Certificate Store</span></span>](#export-encryption-certificate-from-certificate-store)
2. [<span data-ttu-id="a065b-171">Töltse fel a titkosítási tanúsítvány tooCloud szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a065b-171">Upload Encryption Certificate tooCloud Service</span></span>](#upload-encryption-certificate-to-cloud-service)
3. [<span data-ttu-id="a065b-172">A szolgáltatás konfigurációs fájljában titkosítási tanúsítvány frissítése</span><span class="sxs-lookup"><span data-stu-id="a065b-172">Update Encryption Certificate in Service Configuration File</span></span>](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a><span data-ttu-id="a065b-173">Létező tanúsítvány használatára a PFX-fájl</span><span class="sxs-lookup"><span data-stu-id="a065b-173">Use an existing certificate in a PFX file</span></span>
1. [<span data-ttu-id="a065b-174">Töltse fel a titkosítási tanúsítvány tooCloud szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a065b-174">Upload Encryption Certificate tooCloud Service</span></span>](#upload-encryption-certificate-to-cloud-service)
2. [<span data-ttu-id="a065b-175">A szolgáltatás konfigurációs fájljában titkosítási tanúsítvány frissítése</span><span class="sxs-lookup"><span data-stu-id="a065b-175">Update Encryption Certificate in Service Configuration File</span></span>](#update-encryption-certificate-in-service-configuration-file)

## <a name="hello-default-configuration"></a><span data-ttu-id="a065b-176">hello alapértelmezett konfigurációja</span><span class="sxs-lookup"><span data-stu-id="a065b-176">hello default configuration</span></span>
<span data-ttu-id="a065b-177">hello alapértelmezett konfiguráció összes hozzáférési toohello HTTP-végpont megtagadja.</span><span class="sxs-lookup"><span data-stu-id="a065b-177">hello default configuration denies all access toohello HTTP endpoint.</span></span> <span data-ttu-id="a065b-178">Ez a beállítás, javasolt, mert hello kérelmek toothese végpontok is elvégezheti a bizalmas információkat, például adatbázis-hitelesítő adatok hello.</span><span class="sxs-lookup"><span data-stu-id="a065b-178">This is hello recommended setting, since hello requests toothese endpoints may carry sensitive information like database credentials.</span></span>
<span data-ttu-id="a065b-179">hello alapértelmezett konfigurációja lehetővé teszi, hogy minden hozzáférési toohello HTTPS-végponton.</span><span class="sxs-lookup"><span data-stu-id="a065b-179">hello default configuration allows all access toohello HTTPS endpoint.</span></span> <span data-ttu-id="a065b-180">Ezzel a beállítással korlátozható tovább.</span><span class="sxs-lookup"><span data-stu-id="a065b-180">This setting may be restricted further.</span></span>

### <a name="changing-hello-configuration"></a><span data-ttu-id="a065b-181">Hello konfiguráció módosítása</span><span class="sxs-lookup"><span data-stu-id="a065b-181">Changing hello Configuration</span></span>
<span data-ttu-id="a065b-182">hello tooand végpont vonatkozó hozzáférés-vezérlési szabályok csoportja konfigurált hello  **<EndpointAcls>**  hello szakasz **szolgáltatás konfigurációs fájlja**.</span><span class="sxs-lookup"><span data-stu-id="a065b-182">hello group of access control rules that apply tooand endpoint are configured in hello **<EndpointAcls>** section in hello **service configuration file**.</span></span>

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

<span data-ttu-id="a065b-183">hozzáférés-vezérlési csoportban hello szabályok úgy vannak konfigurálva, a egy <AccessControl name=""> hello szolgáltatás konfigurációs fájl részét.</span><span class="sxs-lookup"><span data-stu-id="a065b-183">hello rules in an access control group are configured in a <AccessControl name=""> section of hello service configuration file.</span></span> 

<span data-ttu-id="a065b-184">hello formátum esetén, tekintse meg a hálózati hozzáférés-vezérlési listái dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="a065b-184">hello format is explained in Network Access Control Lists documentation.</span></span>
<span data-ttu-id="a065b-185">Például tooallow egyetlen IP-címek a hello tartomány 100.100.0.0 too100.100.255.255 tooaccess hello HTTPS-végponton, hello szabályok néz ki:</span><span class="sxs-lookup"><span data-stu-id="a065b-185">For example, tooallow only IPs in hello range 100.100.0.0 too100.100.255.255 tooaccess hello HTTPS endpoint, hello rules would look like this:</span></span>

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a><span data-ttu-id="a065b-186">Letiltja a szolgáltatás megelőzése</span><span class="sxs-lookup"><span data-stu-id="a065b-186">Denial of service prevention</span></span>
<span data-ttu-id="a065b-187">Két különböző mechanizmus toodetect támogatott, és szolgáltatásmegtagadási támadások megelőzése érdekében:</span><span class="sxs-lookup"><span data-stu-id="a065b-187">There are two different mechanisms supported toodetect and prevent Denial of Service attacks:</span></span>

* <span data-ttu-id="a065b-188">Korlátozhatja a távoli állomásonként egyidejű kérelmek száma (alapértelmezés szerint kikapcsolva)</span><span class="sxs-lookup"><span data-stu-id="a065b-188">Restrict number of concurrent requests per remote host (off by default)</span></span>
* <span data-ttu-id="a065b-189">Távoli állomásonként hozzáférési sebesség korlátozása (az alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="a065b-189">Restrict rate of access per remote host (on by default)</span></span>

<span data-ttu-id="a065b-190">Ezek a további részletes ismertetését lásd: az IIS-ben a dinamikus IP-biztonság hello szolgáltatások alapulnak.</span><span class="sxs-lookup"><span data-stu-id="a065b-190">These are based on hello features further documented in Dynamic IP Security in IIS.</span></span> <span data-ttu-id="a065b-191">Ha ez a konfiguráció módosítása figyeljen a hello a következő tényezőket:</span><span class="sxs-lookup"><span data-stu-id="a065b-191">When changing this configuration beware of hello following factors:</span></span>

* <span data-ttu-id="a065b-192">proxyk és a hálózati címfordítás eszközökön keresztüli hello távoli állomásadatai hello viselkedését</span><span class="sxs-lookup"><span data-stu-id="a065b-192">hello behavior of proxies and Network Address Translation devices over hello remote host information</span></span>
* <span data-ttu-id="a065b-193">Minden egyes kérelem tooany erőforrás hello webes szerepkörben tekinthető (pl. betöltése parancsfájlok, képek, stb)</span><span class="sxs-lookup"><span data-stu-id="a065b-193">Each request tooany resource in hello web role is considered (e.g. loading scripts, images, etc)</span></span>

## <a name="restricting-number-of-concurrent-accesses"></a><span data-ttu-id="a065b-194">Korlátozza az egyidejű hozzáférések száma</span><span class="sxs-lookup"><span data-stu-id="a065b-194">Restricting number of concurrent accesses</span></span>
<span data-ttu-id="a065b-195">Ez a viselkedés konfigurált hello-beállítások a következők:</span><span class="sxs-lookup"><span data-stu-id="a065b-195">hello settings that configure this behavior are:</span></span>

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

<span data-ttu-id="a065b-196">Ez a védelem DynamicIpRestrictionDenyByConcurrentRequests tootrue tooenable módosítása</span><span class="sxs-lookup"><span data-stu-id="a065b-196">Change DynamicIpRestrictionDenyByConcurrentRequests tootrue tooenable this protection.</span></span>

## <a name="restricting-rate-of-access"></a><span data-ttu-id="a065b-197">Gyakorisága a hozzáférés korlátozása</span><span class="sxs-lookup"><span data-stu-id="a065b-197">Restricting rate of access</span></span>
<span data-ttu-id="a065b-198">Ez a viselkedés konfigurált hello-beállítások a következők:</span><span class="sxs-lookup"><span data-stu-id="a065b-198">hello settings that configure this behavior are:</span></span>

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-hello-response-tooa-denied-request"></a><span data-ttu-id="a065b-199">Hello válasz tooa konfigurálása elutasította a kérelmet</span><span class="sxs-lookup"><span data-stu-id="a065b-199">Configuring hello response tooa denied request</span></span>
<span data-ttu-id="a065b-200">hello következő beállítással hello válasz tooa elutasította a kérelmet:</span><span class="sxs-lookup"><span data-stu-id="a065b-200">hello following setting configures hello response tooa denied request:</span></span>

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
<span data-ttu-id="a065b-201">Tekintse meg a toohello dokumentáció dinamikus IP-biztonság az IIS-ben más támogatott értékek.</span><span class="sxs-lookup"><span data-stu-id="a065b-201">Refer toohello documentation for Dynamic IP Security in IIS for other supported values.</span></span>

## <a name="operations-for-configuring-service-certificates"></a><span data-ttu-id="a065b-202">Műveletek a szolgáltatás tanúsítványok konfigurálásáról</span><span class="sxs-lookup"><span data-stu-id="a065b-202">Operations for configuring service certificates</span></span>
<span data-ttu-id="a065b-203">Ez a témakör csak a hivatkozás van.</span><span class="sxs-lookup"><span data-stu-id="a065b-203">This topic is for reference only.</span></span> <span data-ttu-id="a065b-204">Hajtsa végre hello leírt konfigurációs lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a065b-204">Please follow hello configuration steps outlined in:</span></span>

* <span data-ttu-id="a065b-205">Hello SSL-tanúsítvány konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a065b-205">Configure hello SSL certificate</span></span>
* <span data-ttu-id="a065b-206">Állítson be ügyféltanúsítványokat</span><span class="sxs-lookup"><span data-stu-id="a065b-206">Configure client certificates</span></span>

## <a name="create-a-self-signed-certificate"></a><span data-ttu-id="a065b-207">Önaláírt tanúsítvány létrehozása</span><span class="sxs-lookup"><span data-stu-id="a065b-207">Create a self-signed certificate</span></span>
<span data-ttu-id="a065b-208">Hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="a065b-208">Execute:</span></span>

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

<span data-ttu-id="a065b-209">toocustomize:</span><span class="sxs-lookup"><span data-stu-id="a065b-209">toocustomize:</span></span>

* <span data-ttu-id="a065b-210">-n hello szolgáltatási URL-cím.</span><span class="sxs-lookup"><span data-stu-id="a065b-210">-n with hello service URL.</span></span> <span data-ttu-id="a065b-211">Helyettesítő karakterek ("CN = * .cloudapp .net") és az alternatív neveket ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net") támogatottak.</span><span class="sxs-lookup"><span data-stu-id="a065b-211">Wildcards ("CN=*.cloudapp.net") and alternative names ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net") are supported.</span></span>
* <span data-ttu-id="a065b-212">-e a hello tanúsítvány lejárati dátuma hozzon létre egy erős jelszót, és adja meg, amikor a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="a065b-212">-e with hello certificate expiration date Create a strong password and specify it when prompted.</span></span>

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a><span data-ttu-id="a065b-213">Hozzon létre önaláírt SSL-tanúsítvány PFX-fájlt</span><span class="sxs-lookup"><span data-stu-id="a065b-213">Create PFX file for self-signed SSL certificate</span></span>
<span data-ttu-id="a065b-214">Hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="a065b-214">Execute:</span></span>

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

<span data-ttu-id="a065b-215">Adja meg a jelszót, és exportálhatja a tanúsítványt, amelynek ezeket a beállításokat:</span><span class="sxs-lookup"><span data-stu-id="a065b-215">Enter password and then export certificate with these options:</span></span>

* <span data-ttu-id="a065b-216">Igen, a hello titkos kulcs exportálása</span><span class="sxs-lookup"><span data-stu-id="a065b-216">Yes, export hello private key</span></span>
* <span data-ttu-id="a065b-217">Minden további tulajdonság exportálása</span><span class="sxs-lookup"><span data-stu-id="a065b-217">Export all extended properties</span></span>

## <a name="export-ssl-certificate-from-certificate-store"></a><span data-ttu-id="a065b-218">SSL-tanúsítvány exportálása a tanúsítványt a tanúsítványtárolóból</span><span class="sxs-lookup"><span data-stu-id="a065b-218">Export SSL certificate from certificate store</span></span>
* <span data-ttu-id="a065b-219">Tanúsítvány található</span><span class="sxs-lookup"><span data-stu-id="a065b-219">Find certificate</span></span>
* <span data-ttu-id="a065b-220">Kattintson a műveletek összes -> feladatok -> Exportálás...</span><span class="sxs-lookup"><span data-stu-id="a065b-220">Click Actions -> All tasks -> Export…</span></span>
* <span data-ttu-id="a065b-221">Exportálja a tanúsítványt egy. Ezek a beállítások a PFX-fájlt:</span><span class="sxs-lookup"><span data-stu-id="a065b-221">Export certificate into a .PFX file with these options:</span></span>
  * <span data-ttu-id="a065b-222">Igen, a hello titkos kulcs exportálása</span><span class="sxs-lookup"><span data-stu-id="a065b-222">Yes, export hello private key</span></span>
  * <span data-ttu-id="a065b-223">Minden tanúsítvány belefoglalása a tanúsítványláncba, hello lehetőleg * minden további tulajdonság exportálása</span><span class="sxs-lookup"><span data-stu-id="a065b-223">Include all certificates in hello certification path if possible *Export all extended properties</span></span>

## <a name="upload-ssl-certificate-toocloud-service"></a><span data-ttu-id="a065b-224">Töltse fel az SSL-tanúsítvány toocloud szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a065b-224">Upload SSL certificate toocloud service</span></span>
<span data-ttu-id="a065b-225">Feltöltés a hello meglévő tanúsítványt, vagy jön létre. Az SSL-kulcsból álló kulcspárt hello PFX-fájlt:</span><span class="sxs-lookup"><span data-stu-id="a065b-225">Upload certificate with hello existing or generated .PFX file with hello SSL key pair:</span></span>

* <span data-ttu-id="a065b-226">Adja meg a hello jelszó hello titkos kulcs adatainak védelme</span><span class="sxs-lookup"><span data-stu-id="a065b-226">Enter hello password protecting hello private key information</span></span>

## <a name="update-ssl-certificate-in-service-configuration-file"></a><span data-ttu-id="a065b-227">A szolgáltatás konfigurációs fájljában SSL-tanúsítvány frissítése</span><span class="sxs-lookup"><span data-stu-id="a065b-227">Update SSL certificate in service configuration file</span></span>
<span data-ttu-id="a065b-228">A következő beállítás hello szolgáltatás konfigurációs fájljában hello feltöltött tanúsítvány toohello felhőszolgáltatás hello ujjlenyomattal rendelkező hello hello ujjlenyomat értékének frissítése:</span><span class="sxs-lookup"><span data-stu-id="a065b-228">Update hello thumbprint value of hello following setting in hello service configuration file with hello thumbprint of hello certificate uploaded toohello cloud service:</span></span>

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a><span data-ttu-id="a065b-229">SSL-hitelesítésszolgáltató importálása</span><span class="sxs-lookup"><span data-stu-id="a065b-229">Import SSL certification authority</span></span>
<span data-ttu-id="a065b-230">Kövesse az alábbi lépéseket az összes fiók/gépen hello szolgáltatással kommunikáló:</span><span class="sxs-lookup"><span data-stu-id="a065b-230">Follow these steps in all account/machine that will communicate with hello service:</span></span>

* <span data-ttu-id="a065b-231">Kattintson duplán a hello. A Windows Intézőben CER-fájljával</span><span class="sxs-lookup"><span data-stu-id="a065b-231">Double-click hello .CER file in Windows Explorer</span></span>
* <span data-ttu-id="a065b-232">Hello tanúsítvány párbeszédpanelen kattintson a tanúsítvány telepítése...</span><span class="sxs-lookup"><span data-stu-id="a065b-232">In hello Certificate dialog, click Install Certificate…</span></span>
* <span data-ttu-id="a065b-233">Importálja a tanúsítványt a megbízható legfelső szintű hitelesítésszolgáltatók tárolására hello</span><span class="sxs-lookup"><span data-stu-id="a065b-233">Import certificate into hello Trusted Root Certification Authorities store</span></span>

## <a name="turn-off-client-certificate-based-authentication"></a><span data-ttu-id="a065b-234">Ügyféltanúsítvány-alapú hitelesítés kikapcsolása</span><span class="sxs-lookup"><span data-stu-id="a065b-234">Turn off client certificate-based authentication</span></span>
<span data-ttu-id="a065b-235">Csak az ügyféltanúsítvány-alapú hitelesítés támogatott, és letiltja azt lehetővé teszi a nyilvános hozzáférés toohello Szolgáltatásvégpontok, kivéve, ha más mechanizmusok helyen (pl. Microsoft Azure virtuális hálózat).</span><span class="sxs-lookup"><span data-stu-id="a065b-235">Only client certificate-based authentication is supported and disabling it will allow for public access toohello service endpoints, unless other mechanisms are in place (e.g. Microsoft Azure Virtual Network).</span></span>

<span data-ttu-id="a065b-236">Módosítsa ezen beállítások toofalse hello szolgáltatás konfigurációs fájl tooturn hello szolgáltatásban:</span><span class="sxs-lookup"><span data-stu-id="a065b-236">Change these settings toofalse in hello service configuration file tooturn hello feature off:</span></span>

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

<span data-ttu-id="a065b-237">Ezután másolja ugyanazzal az ujjlenyomattal mint hello SSL-tanúsítvány hello hello Hitelesítésszolgáltatói tanúsítvány beállítása:</span><span class="sxs-lookup"><span data-stu-id="a065b-237">Then, copy hello same thumbprint as hello SSL certificate in hello CA certificate setting:</span></span>

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a><span data-ttu-id="a065b-238">Hozzon létre egy önaláírt hitelesítésszolgáltató</span><span class="sxs-lookup"><span data-stu-id="a065b-238">Create a self-signed certification authority</span></span>
<span data-ttu-id="a065b-239">A következő lépéseket toocreate egy önaláírt tanúsítványt tooact hitelesítésszolgáltatóként hello hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="a065b-239">Execute hello following steps toocreate a self-signed certificate tooact as a Certification Authority:</span></span>

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

<span data-ttu-id="a065b-240">toocustomize azt</span><span class="sxs-lookup"><span data-stu-id="a065b-240">toocustomize it</span></span>

* <span data-ttu-id="a065b-241">-e a hello tanúsítvány lejárati dátuma</span><span class="sxs-lookup"><span data-stu-id="a065b-241">-e with hello certification expiration date</span></span>

## <a name="find-ca-public-key"></a><span data-ttu-id="a065b-242">Nyilvános hitelesítésszolgáltató-kulcs megkeresése</span><span class="sxs-lookup"><span data-stu-id="a065b-242">Find CA public key</span></span>
<span data-ttu-id="a065b-243">Minden ügyfél kell kiadott tanúsítványok hello szolgáltatás megbízható hitelesítésszolgáltató által.</span><span class="sxs-lookup"><span data-stu-id="a065b-243">All client certificates must have been issued by a Certification Authority trusted by hello service.</span></span> <span data-ttu-id="a065b-244">Található hello nyilvános kulcs toohello toobe fog hello ügyféltanúsítványok kiállító hitelesítésszolgáltató-hitelesítéshez használt a rendelés tooupload azt toohello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a065b-244">Find hello public key toohello Certification Authority that issued hello client certificates that are going toobe used for authentication in order tooupload it toohello cloud service.</span></span>

<span data-ttu-id="a065b-245">Hello nyilvános kulccsal hello fájl nem érhető el, ha exportálja hello tanúsítványtárolóból:</span><span class="sxs-lookup"><span data-stu-id="a065b-245">If hello file with hello public key is not available, export it from hello certificate store:</span></span>

* <span data-ttu-id="a065b-246">Tanúsítvány található</span><span class="sxs-lookup"><span data-stu-id="a065b-246">Find certificate</span></span>
  * <span data-ttu-id="a065b-247">Keresse meg az ügyfél által kiállított tanúsítványt hello ugyanazt a hitelesítésszolgáltatót</span><span class="sxs-lookup"><span data-stu-id="a065b-247">Search for a client certificate issued by hello same Certification Authority</span></span>
* <span data-ttu-id="a065b-248">Kattintson duplán a hello tanúsítványra.</span><span class="sxs-lookup"><span data-stu-id="a065b-248">Double-click hello certificate.</span></span>
* <span data-ttu-id="a065b-249">Válassza ki a hello az Általános lapon hello tanúsítvány párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="a065b-249">Select hello Certification Path tab in hello Certificate dialog.</span></span>
* <span data-ttu-id="a065b-250">Kattintson duplán hello hitelesítésszolgáltató hello elérési úton.</span><span class="sxs-lookup"><span data-stu-id="a065b-250">Double-click hello CA entry in hello path.</span></span>
* <span data-ttu-id="a065b-251">Készítsen feljegyzéseket hello tanúsítvány tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="a065b-251">Take notes of hello certificate properties.</span></span>
* <span data-ttu-id="a065b-252">Bezárás hello **tanúsítvány** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a065b-252">Close hello **Certificate** dialog.</span></span>
* <span data-ttu-id="a065b-253">Tanúsítvány található</span><span class="sxs-lookup"><span data-stu-id="a065b-253">Find certificate</span></span>
  * <span data-ttu-id="a065b-254">Keresse meg a fent leírt hitelesítésszolgáltató hello.</span><span class="sxs-lookup"><span data-stu-id="a065b-254">Search for hello CA noted above.</span></span>
* <span data-ttu-id="a065b-255">Kattintson a műveletek összes -> feladatok -> Exportálás...</span><span class="sxs-lookup"><span data-stu-id="a065b-255">Click Actions -> All tasks -> Export…</span></span>
* <span data-ttu-id="a065b-256">Exportálja a tanúsítványt egy. CER beállítások segítségével:</span><span class="sxs-lookup"><span data-stu-id="a065b-256">Export certificate into a .CER with these options:</span></span>
  * <span data-ttu-id="a065b-257">**Nem, nem exportálja a titkos kulcs hello**</span><span class="sxs-lookup"><span data-stu-id="a065b-257">**No, do not export hello private key**</span></span>
  * <span data-ttu-id="a065b-258">Minden tanúsítvány belefoglalása hello tanúsítványláncba, ha lehetséges.</span><span class="sxs-lookup"><span data-stu-id="a065b-258">Include all certificates in hello certification path if possible.</span></span>
  * <span data-ttu-id="a065b-259">Minden további tulajdonság exportálása.</span><span class="sxs-lookup"><span data-stu-id="a065b-259">Export all extended properties.</span></span>

## <a name="upload-ca-certificate-toocloud-service"></a><span data-ttu-id="a065b-260">Töltse fel a Hitelesítésszolgáltatói tanúsítvány toocloud szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a065b-260">Upload CA certificate toocloud service</span></span>
<span data-ttu-id="a065b-261">Feltöltés a hello meglévő tanúsítványt, vagy jön létre. CER-fájljával hello hitelesítésszolgáltató nyilvános kulccsal.</span><span class="sxs-lookup"><span data-stu-id="a065b-261">Upload certificate with hello existing or generated .CER file with hello CA public key.</span></span>

## <a name="update-ca-certificate-in-service-configuration-file"></a><span data-ttu-id="a065b-262">A szolgáltatás konfigurációs fájljában frissítés Hitelesítésszolgáltatói tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="a065b-262">Update CA certificate in service configuration file</span></span>
<span data-ttu-id="a065b-263">A következő beállítás hello szolgáltatás konfigurációs fájljában hello feltöltött tanúsítvány toohello felhőszolgáltatás hello ujjlenyomattal rendelkező hello hello ujjlenyomat értékének frissítése:</span><span class="sxs-lookup"><span data-stu-id="a065b-263">Update hello thumbprint value of hello following setting in hello service configuration file with hello thumbprint of hello certificate uploaded toohello cloud service:</span></span>

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

<span data-ttu-id="a065b-264">A következő beállítás a hello azonos hello hello értékét ujjlenyomata:</span><span class="sxs-lookup"><span data-stu-id="a065b-264">Update hello value of hello following setting with hello same thumbprint:</span></span>

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a><span data-ttu-id="a065b-265">Ügyfél tanúsítványokat</span><span class="sxs-lookup"><span data-stu-id="a065b-265">Issue client certificates</span></span>
<span data-ttu-id="a065b-266">Minden egyes jogosult tooaccess hello szolgáltatás ügyféltanúsítvánnyal a his/hers kizárólagos használatára kell lennie, és válasszon his/hers saját tooprotect erős jelszót a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="a065b-266">Each individual authorized tooaccess hello service should have a client certificate issued for his/hers exclusive use and should choose his/hers own strong password tooprotect its private key.</span></span> 

<span data-ttu-id="a065b-267">egyazon számítógépen, ahol hello önaláírt hitelesítésszolgáltató tanúsítványát generált és a tárolt hello hello a következő lépéseket kell végrehajtani:</span><span class="sxs-lookup"><span data-stu-id="a065b-267">hello following steps must be executed in hello same machine where hello self-signed CA certificate was generated and stored:</span></span>

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

<span data-ttu-id="a065b-268">Testreszabása:</span><span class="sxs-lookup"><span data-stu-id="a065b-268">Customizing:</span></span>

* <span data-ttu-id="a065b-269">Ezzel a tanúsítvánnyal hitelesítése toohello ügyfélnek azonosítójú -n</span><span class="sxs-lookup"><span data-stu-id="a065b-269">-n with an ID for toohello client that will be authenticated with this certificate</span></span>
* <span data-ttu-id="a065b-270">-e a hello tanúsítvány lejárati dátuma</span><span class="sxs-lookup"><span data-stu-id="a065b-270">-e with hello certificate expiration date</span></span>
* <span data-ttu-id="a065b-271">MyID.pvk és MyID.cer, a egyedi fájlneveket az ügyféltanúsítványt</span><span class="sxs-lookup"><span data-stu-id="a065b-271">MyID.pvk and MyID.cer with unique filenames for this client certificate</span></span>

<span data-ttu-id="a065b-272">Ez a parancs felszólítja egy jelszó toobe létrehozott, és egyszer használva.</span><span class="sxs-lookup"><span data-stu-id="a065b-272">This command will prompt for a password toobe created and then used once.</span></span> <span data-ttu-id="a065b-273">Használjon erős jelszót.</span><span class="sxs-lookup"><span data-stu-id="a065b-273">Use a strong password.</span></span>

## <a name="create-pfx-files-for-client-certificates"></a><span data-ttu-id="a065b-274">Az ügyfél PFX-fájlok tanúsítványok létrehozása</span><span class="sxs-lookup"><span data-stu-id="a065b-274">Create PFX files for client certificates</span></span>
<span data-ttu-id="a065b-275">Minden létrehozott ügyféltanúsítványt hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="a065b-275">For each generated client certificate, execute:</span></span>

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

<span data-ttu-id="a065b-276">Testreszabása:</span><span class="sxs-lookup"><span data-stu-id="a065b-276">Customizing:</span></span>

    MyID.pvk and MyID.cer with hello filename for hello client certificate

<span data-ttu-id="a065b-277">Adja meg a jelszót, és exportálhatja a tanúsítványt, amelynek ezeket a beállításokat:</span><span class="sxs-lookup"><span data-stu-id="a065b-277">Enter password and then export certificate with these options:</span></span>

* <span data-ttu-id="a065b-278">Igen, a hello titkos kulcs exportálása</span><span class="sxs-lookup"><span data-stu-id="a065b-278">Yes, export hello private key</span></span>
* <span data-ttu-id="a065b-279">Minden további tulajdonság exportálása</span><span class="sxs-lookup"><span data-stu-id="a065b-279">Export all extended properties</span></span>
* <span data-ttu-id="a065b-280">Ez a tanúsítvány kiállítása történik hello egyedi toowhom válasszon hello exportálási jelszó</span><span class="sxs-lookup"><span data-stu-id="a065b-280">hello individual toowhom this certificate is being issued should choose hello export password</span></span>

## <a name="import-client-certificate"></a><span data-ttu-id="a065b-281">Ügyfél-tanúsítvány importálása</span><span class="sxs-lookup"><span data-stu-id="a065b-281">Import client certificate</span></span>
<span data-ttu-id="a065b-282">Minden egyes személy, amely ügyféltanúsítványt bocsátották hello kulcsból álló kulcspárt kell importálni a hello gépek többé használ toocommunicate hello szolgáltatásban:</span><span class="sxs-lookup"><span data-stu-id="a065b-282">Each individual for whom a client certificate has been issued should import hello key pair in hello machines he/she will use toocommunicate with hello service:</span></span>

* <span data-ttu-id="a065b-283">Kattintson duplán a hello. PFX-fájlt a Windows Intézőben</span><span class="sxs-lookup"><span data-stu-id="a065b-283">Double-click hello .PFX file in Windows Explorer</span></span>
* <span data-ttu-id="a065b-284">Tanúsítvány importálása hello személyes tárolójából rendelkező legalább ezt a beállítást:</span><span class="sxs-lookup"><span data-stu-id="a065b-284">Import certificate into hello Personal store with at least this option:</span></span>
  * <span data-ttu-id="a065b-285">Minden további tulajdonság be van jelölve szerepeltetése</span><span class="sxs-lookup"><span data-stu-id="a065b-285">Include all extended properties checked</span></span>

## <a name="copy-client-certificate-thumbprints"></a><span data-ttu-id="a065b-286">Másolja a tanúsítvány-ujjlenyomatok client</span><span class="sxs-lookup"><span data-stu-id="a065b-286">Copy client certificate thumbprints</span></span>
<span data-ttu-id="a065b-287">Minden egyes személy, amely ügyféltanúsítványt bocsátották kövesse az alábbi lépéseket a rendelés tooobtain hello ujjlenyomata his/hers tanúsítvány, amely belekerül toohello szolgáltatás konfigurációs fájljában:</span><span class="sxs-lookup"><span data-stu-id="a065b-287">Each individual for whom a client certificate has been issued must follow these steps in order tooobtain hello thumbprint of his/hers certificate which will be added toohello service configuration file:</span></span>

* <span data-ttu-id="a065b-288">Certmgr.exe futtatása</span><span class="sxs-lookup"><span data-stu-id="a065b-288">Run certmgr.exe</span></span>
* <span data-ttu-id="a065b-289">Válassza ki a hello személyes lap</span><span class="sxs-lookup"><span data-stu-id="a065b-289">Select hello Personal tab</span></span>
* <span data-ttu-id="a065b-290">Kattintson duplán a hitelesítéshez használt toobe hello ügyféltanúsítványt</span><span class="sxs-lookup"><span data-stu-id="a065b-290">Double-click hello client certificate toobe used for authentication</span></span>
* <span data-ttu-id="a065b-291">Hello tanúsítvány megnyíló párbeszédpanelen válassza ki hello részletei lapon.</span><span class="sxs-lookup"><span data-stu-id="a065b-291">In hello Certificate dialog that opens, select hello Details tab</span></span>
* <span data-ttu-id="a065b-292">Győződjön meg arról, hogy megjeleníti az összes megjelenítése</span><span class="sxs-lookup"><span data-stu-id="a065b-292">Make sure Show is displaying All</span></span>
* <span data-ttu-id="a065b-293">Ujjlenyomat nevű hello listában válassza hello mező</span><span class="sxs-lookup"><span data-stu-id="a065b-293">Select hello field named Thumbprint in hello list</span></span>
* <span data-ttu-id="a065b-294">Hello ujjlenyomat hello értéket másol ** törlése előtt hello első számjegy láthatatlan Unicode-karaktereket ** csak szóközökből törlése</span><span class="sxs-lookup"><span data-stu-id="a065b-294">Copy hello value of hello thumbprint ** Delete non-visible Unicode characters in front of hello first digit ** Delete all spaces</span></span>

## <a name="configure-allowed-clients-in-hello-service-configuration-file"></a><span data-ttu-id="a065b-295">Hello szolgáltatás konfigurációs fájljában engedélyezett ügyfelek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a065b-295">Configure Allowed clients in hello service configuration file</span></span>
<span data-ttu-id="a065b-296">A következő beállítás hello szolgáltatás konfigurációs fájljában toohello szolgáltatás engedélyezett hello ügyféltanúsítványok hello ujjlenyomatai vesszővel tagolt listáját tartalmazó hello hello értékének frissítése:</span><span class="sxs-lookup"><span data-stu-id="a065b-296">Update hello value of hello following setting in hello service configuration file with a comma-separated list of hello thumbprints of hello client certificates allowed access toohello service:</span></span>

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a><span data-ttu-id="a065b-297">Konfigurálja az ügyfél visszavonási állapotának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="a065b-297">Configure client certificate revocation check</span></span>
<span data-ttu-id="a065b-298">hello alapértelmezett beállítása nem ellenőrzi a hitelesítésszolgáltató hello az ügyfél tanúsítvány-visszavonási állapotát.</span><span class="sxs-lookup"><span data-stu-id="a065b-298">hello default setting does not check with hello Certification Authority for client certificate revocation status.</span></span> <span data-ttu-id="a065b-299">a hello tooturn ellenőrzi, ha hello hello tanúsítványt kiállító hitelesítésszolgáltató támogatja az ilyen ellenőrzéseket, módosítsa a beállítást követő hello X509RevocationMode számbavételi definiált hello értékek egyikével hello:</span><span class="sxs-lookup"><span data-stu-id="a065b-299">tooturn on hello checks, if hello Certification Authority which issued hello client certificates supports such checks, change hello following setting with one of hello values defined in hello X509RevocationMode Enumeration:</span></span>

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a><span data-ttu-id="a065b-300">Hozzon létre önaláírt titkosítási tanúsítványok PFX-fájlt</span><span class="sxs-lookup"><span data-stu-id="a065b-300">Create PFX file for self-signed encryption certificates</span></span>
<span data-ttu-id="a065b-301">Titkosítási tanúsítványt az alábbi hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="a065b-301">For an encryption certificate, execute:</span></span>

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

<span data-ttu-id="a065b-302">Testreszabása:</span><span class="sxs-lookup"><span data-stu-id="a065b-302">Customizing:</span></span>

    MyID.pvk and MyID.cer with hello filename for hello encryption certificate

<span data-ttu-id="a065b-303">Adja meg a jelszót, és exportálhatja a tanúsítványt, amelynek ezeket a beállításokat:</span><span class="sxs-lookup"><span data-stu-id="a065b-303">Enter password and then export certificate with these options:</span></span>

* <span data-ttu-id="a065b-304">Igen, a hello titkos kulcs exportálása</span><span class="sxs-lookup"><span data-stu-id="a065b-304">Yes, export hello private key</span></span>
* <span data-ttu-id="a065b-305">Minden további tulajdonság exportálása</span><span class="sxs-lookup"><span data-stu-id="a065b-305">Export all extended properties</span></span>
* <span data-ttu-id="a065b-306">Hello jelszót kell hello tanúsítvány toohello felhőszolgáltatás feltöltésekor.</span><span class="sxs-lookup"><span data-stu-id="a065b-306">You will need hello password when uploading hello certificate toohello cloud service.</span></span>

## <a name="export-encryption-certificate-from-certificate-store"></a><span data-ttu-id="a065b-307">Titkosítási tanúsítvány exportálása a tanúsítványt a tanúsítványtárolóból</span><span class="sxs-lookup"><span data-stu-id="a065b-307">Export encryption certificate from certificate store</span></span>
* <span data-ttu-id="a065b-308">Tanúsítvány található</span><span class="sxs-lookup"><span data-stu-id="a065b-308">Find certificate</span></span>
* <span data-ttu-id="a065b-309">Kattintson a műveletek összes -> feladatok -> Exportálás...</span><span class="sxs-lookup"><span data-stu-id="a065b-309">Click Actions -> All tasks -> Export…</span></span>
* <span data-ttu-id="a065b-310">Exportálja a tanúsítványt egy. Ezek a beállítások a PFX-fájlt:</span><span class="sxs-lookup"><span data-stu-id="a065b-310">Export certificate into a .PFX file with these options:</span></span> 
  * <span data-ttu-id="a065b-311">Igen, a hello titkos kulcs exportálása</span><span class="sxs-lookup"><span data-stu-id="a065b-311">Yes, export hello private key</span></span>
  * <span data-ttu-id="a065b-312">Minden tanúsítvány belefoglalása a tanúsítványláncba, hello Ha lehetséges</span><span class="sxs-lookup"><span data-stu-id="a065b-312">Include all certificates in hello certification path if possible</span></span> 
* <span data-ttu-id="a065b-313">Minden további tulajdonság exportálása</span><span class="sxs-lookup"><span data-stu-id="a065b-313">Export all extended properties</span></span>

## <a name="upload-encryption-certificate-toocloud-service"></a><span data-ttu-id="a065b-314">Töltse fel a titkosítási tanúsítvány toocloud szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a065b-314">Upload encryption certificate toocloud service</span></span>
<span data-ttu-id="a065b-315">Feltöltés a hello meglévő tanúsítványt, vagy jön létre. Hello titkosítási kulcsból álló kulcspárt a PFX-fájlt:</span><span class="sxs-lookup"><span data-stu-id="a065b-315">Upload certificate with hello existing or generated .PFX file with hello encryption key pair:</span></span>

* <span data-ttu-id="a065b-316">Adja meg a hello jelszó hello titkos kulcs adatainak védelme</span><span class="sxs-lookup"><span data-stu-id="a065b-316">Enter hello password protecting hello private key information</span></span>

## <a name="update-encryption-certificate-in-service-configuration-file"></a><span data-ttu-id="a065b-317">A szolgáltatás konfigurációs fájljában titkosítási tanúsítvány frissítése</span><span class="sxs-lookup"><span data-stu-id="a065b-317">Update encryption certificate in service configuration file</span></span>
<span data-ttu-id="a065b-318">A következő beállítások hello szolgáltatás konfigurációs fájljában hello feltöltött tanúsítvány toohello felhőszolgáltatás hello ujjlenyomattal rendelkező hello hello ujjlenyomat értékének frissítése:</span><span class="sxs-lookup"><span data-stu-id="a065b-318">Update hello thumbprint value of hello following settings in hello service configuration file with hello thumbprint of hello certificate uploaded toohello cloud service:</span></span>

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a><span data-ttu-id="a065b-319">Gyakori műveletekhez</span><span class="sxs-lookup"><span data-stu-id="a065b-319">Common certificate operations</span></span>
* <span data-ttu-id="a065b-320">Hello SSL-tanúsítvány konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a065b-320">Configure hello SSL certificate</span></span>
* <span data-ttu-id="a065b-321">Állítson be ügyféltanúsítványokat</span><span class="sxs-lookup"><span data-stu-id="a065b-321">Configure client certificates</span></span>

## <a name="find-certificate"></a><span data-ttu-id="a065b-322">Tanúsítvány található</span><span class="sxs-lookup"><span data-stu-id="a065b-322">Find certificate</span></span>
<span data-ttu-id="a065b-323">Kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a065b-323">Follow these steps:</span></span>

1. <span data-ttu-id="a065b-324">Futtassa a mmc.exe.</span><span class="sxs-lookup"><span data-stu-id="a065b-324">Run mmc.exe.</span></span>
2. <span data-ttu-id="a065b-325">Fájl -> beépülő modul hozzáadása/eltávolítása...</span><span class="sxs-lookup"><span data-stu-id="a065b-325">File -> Add/Remove Snap-in…</span></span>
3. <span data-ttu-id="a065b-326">Válassza ki **tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="a065b-326">Select **Certificates**.</span></span>
4. <span data-ttu-id="a065b-327">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="a065b-327">Click **Add**.</span></span>
5. <span data-ttu-id="a065b-328">Válassza ki a hello tanúsítvány tárolási helyét.</span><span class="sxs-lookup"><span data-stu-id="a065b-328">Choose hello certificate store location.</span></span>
6. <span data-ttu-id="a065b-329">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="a065b-329">Click **Finish**.</span></span>
7. <span data-ttu-id="a065b-330">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a065b-330">Click **OK**.</span></span>
8. <span data-ttu-id="a065b-331">Bontsa ki a **tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="a065b-331">Expand **Certificates**.</span></span>
9. <span data-ttu-id="a065b-332">Bontsa ki a hello tanúsítvány tároló csomópontot.</span><span class="sxs-lookup"><span data-stu-id="a065b-332">Expand hello certificate store node.</span></span>
10. <span data-ttu-id="a065b-333">Bontsa ki a hello tanúsítvány gyermekcsomópontja.</span><span class="sxs-lookup"><span data-stu-id="a065b-333">Expand hello Certificate child node.</span></span>
11. <span data-ttu-id="a065b-334">Jelöljön ki egy tanúsítványt az hello listán.</span><span class="sxs-lookup"><span data-stu-id="a065b-334">Select a certificate in hello list.</span></span>

## <a name="export-certificate"></a><span data-ttu-id="a065b-335">Tanúsítvány exportálása</span><span class="sxs-lookup"><span data-stu-id="a065b-335">Export certificate</span></span>
<span data-ttu-id="a065b-336">A hello **Tanúsítványexportáló varázsló**:</span><span class="sxs-lookup"><span data-stu-id="a065b-336">In hello **Certificate Export Wizard**:</span></span>

1. <span data-ttu-id="a065b-337">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a065b-337">Click **Next**.</span></span>
2. <span data-ttu-id="a065b-338">Válassza ki **Igen**, majd **hello titkos kulcs exportálása**.</span><span class="sxs-lookup"><span data-stu-id="a065b-338">Select **Yes**, then **Export hello private key**.</span></span>
3. <span data-ttu-id="a065b-339">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a065b-339">Click **Next**.</span></span>
4. <span data-ttu-id="a065b-340">Válassza ki a kívánt fájl formátumba hello.</span><span class="sxs-lookup"><span data-stu-id="a065b-340">Select hello desired output file format.</span></span>
5. <span data-ttu-id="a065b-341">Ellenőrizze a szükségeskonfiguráció-hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="a065b-341">Check hello desired options.</span></span>
6. <span data-ttu-id="a065b-342">Ellenőrizze **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a065b-342">Check **Password**.</span></span>
7. <span data-ttu-id="a065b-343">Adjon meg egy erős jelszót, és erősítse meg.</span><span class="sxs-lookup"><span data-stu-id="a065b-343">Enter a strong password and confirm it.</span></span>
8. <span data-ttu-id="a065b-344">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a065b-344">Click **Next**.</span></span>
9. <span data-ttu-id="a065b-345">Írja be vagy keresse meg a fájl nevét, ahol toostore hello tanúsítvány (használja a. PFX-kiterjesztéssel).</span><span class="sxs-lookup"><span data-stu-id="a065b-345">Type or browse a filename where toostore hello certificate (use a .PFX extension).</span></span>
10. <span data-ttu-id="a065b-346">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a065b-346">Click **Next**.</span></span>
11. <span data-ttu-id="a065b-347">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="a065b-347">Click **Finish**.</span></span>
12. <span data-ttu-id="a065b-348">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="a065b-348">Click **OK**.</span></span>

## <a name="import-certificate"></a><span data-ttu-id="a065b-349">Tanúsítvány importálása</span><span class="sxs-lookup"><span data-stu-id="a065b-349">Import certificate</span></span>
<span data-ttu-id="a065b-350">A Tanúsítványimportáló varázsló hello:</span><span class="sxs-lookup"><span data-stu-id="a065b-350">In hello Certificate Import Wizard:</span></span>

1. <span data-ttu-id="a065b-351">Válassza ki a hello tárolóhelyére.</span><span class="sxs-lookup"><span data-stu-id="a065b-351">Select hello store location.</span></span>
   
   * <span data-ttu-id="a065b-352">Válassza ki **aktuális felhasználó** Ha csak az aktuális felhasználó futó folyamatok érik el a hello szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a065b-352">Select **Current User** if only processes running under current user will access hello service</span></span>
   * <span data-ttu-id="a065b-353">Válassza ki **helyi számítógép** Ha a számítógép egyéb folyamatok érik el a hello szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a065b-353">Select **Local Machine** if other processes in this computer will access hello service</span></span>
2. <span data-ttu-id="a065b-354">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a065b-354">Click **Next**.</span></span>
3. <span data-ttu-id="a065b-355">Ha importál egy fájlból, erősítse meg a hello fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="a065b-355">If importing from a file, confirm hello file path.</span></span>
4. <span data-ttu-id="a065b-356">Ha importálni egy. PFX-fájlt:</span><span class="sxs-lookup"><span data-stu-id="a065b-356">If importing a .PFX file:</span></span>
   1. <span data-ttu-id="a065b-357">Adja meg a hello jelszó hello titkos kulcsok védelme</span><span class="sxs-lookup"><span data-stu-id="a065b-357">Enter hello password protecting hello private key</span></span>
   2. <span data-ttu-id="a065b-358">Importálási beállítások megadása</span><span class="sxs-lookup"><span data-stu-id="a065b-358">Select import options</span></span>
5. <span data-ttu-id="a065b-359">Válassza ki a "Hely" tanúsítványok a következő tároló hello</span><span class="sxs-lookup"><span data-stu-id="a065b-359">Select "Place" certificates in hello following store</span></span>
6. <span data-ttu-id="a065b-360">Kattintson a **Browse** (Tallózás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a065b-360">Click **Browse**.</span></span>
7. <span data-ttu-id="a065b-361">Válassza ki a kívánt tárolót hello.</span><span class="sxs-lookup"><span data-stu-id="a065b-361">Select hello desired store.</span></span>
8. <span data-ttu-id="a065b-362">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="a065b-362">Click **Finish**.</span></span>
   
   * <span data-ttu-id="a065b-363">Hello megbízható legfelső szintű hitelesítésszolgáltató-tároló választása esetén kattintson **Igen**.</span><span class="sxs-lookup"><span data-stu-id="a065b-363">If hello Trusted Root Certification Authority store was chosen, click **Yes**.</span></span>
9. <span data-ttu-id="a065b-364">Kattintson a **OK** összes párbeszédpanel windows rendszeren.</span><span class="sxs-lookup"><span data-stu-id="a065b-364">Click **OK** on all dialog windows.</span></span>

## <a name="upload-certificate"></a><span data-ttu-id="a065b-365">Tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="a065b-365">Upload certificate</span></span>
<span data-ttu-id="a065b-366">A hello [Azure portál](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="a065b-366">In hello [Azure Portal](https://portal.azure.com/)</span></span>

1. <span data-ttu-id="a065b-367">Válassza ki **a felhőalapú szolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="a065b-367">Select **Cloud Services**.</span></span>
2. <span data-ttu-id="a065b-368">Válassza ki a hello felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a065b-368">Select hello cloud service.</span></span>
3. <span data-ttu-id="a065b-369">Kattintson a felső menüben hello **tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="a065b-369">On hello top menu, click **Certificates**.</span></span>
4. <span data-ttu-id="a065b-370">Az alsó hello menüsávon kattintson **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="a065b-370">On hello bottom bar, click **Upload**.</span></span>
5. <span data-ttu-id="a065b-371">Válasszon ki hello tanúsítványfájlt.</span><span class="sxs-lookup"><span data-stu-id="a065b-371">Select hello certificate file.</span></span>
6. <span data-ttu-id="a065b-372">Ha a rendszer egy. PFX fájlt, írja be a hello jelszót hello titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="a065b-372">If it is a .PFX file, enter hello password for hello private key.</span></span>
7. <span data-ttu-id="a065b-373">Ezt követően másolása hello új bejegyzést hello lista hello tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="a065b-373">Once completed, copy hello certificate thumbprint from hello new entry in hello list.</span></span>

## <a name="other-security-considerations"></a><span data-ttu-id="a065b-374">Egyéb biztonsági szempontok</span><span class="sxs-lookup"><span data-stu-id="a065b-374">Other security considerations</span></span>
<span data-ttu-id="a065b-375">a jelen dokumentumban ismertetett hello SSL-beállítások hello szolgáltatás és az ügyfelek közötti kommunikáció titkosításához, a HTTPS-végpont hello használata esetén.</span><span class="sxs-lookup"><span data-stu-id="a065b-375">hello SSL settings described in this document encrypt communication between hello service and its clients when hello HTTPS endpoint is used.</span></span> <span data-ttu-id="a065b-376">Ez azért fontos, mert adatbázis-hozzáférési hitelesítő adatok, és más bizalmas adatokat hello kommunikációs tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a065b-376">This is important since credentials for database access and potentially other sensitive information are contained in hello communication.</span></span> <span data-ttu-id="a065b-377">Ne feledje azonban, hogy hello szolgáltatás továbbra is fennáll belső állapotát, a hitelesítő adatokat, beleértve a hello Microsoft Azure SQL-adatbázis a Microsoft Azure-előfizetéshez tartozó metaadat-tároló megadott belső táblájában.</span><span class="sxs-lookup"><span data-stu-id="a065b-377">Note, however, that hello service persists internal status, including credentials, in its internal tables in hello Microsoft Azure SQL database that you have provided for metadata storage in your Microsoft Azure subscription.</span></span> <span data-ttu-id="a065b-378">Beállítás a szolgáltatás konfigurációs fájljában a következő hello részeként definiált, hogy az adatbázis (. CSCFG-fájl):</span><span class="sxs-lookup"><span data-stu-id="a065b-378">That database was defined as part of hello following setting in your service configuration file (.CSCFG file):</span></span> 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

<span data-ttu-id="a065b-379">Ebben az adatbázisban tárolt hitelesítő adatok titkosítása.</span><span class="sxs-lookup"><span data-stu-id="a065b-379">Credentials stored in this database are encrypted.</span></span> <span data-ttu-id="a065b-380">Azonban célszerű, győződjön meg arról, hogy a szolgáltatástelepítések egyaránt webes és feldolgozói szerepkörök tartják be toodate és biztonságos, mint azok egyaránt rendelkezik hozzáféréssel toohello metaadatok adatbázis és a hello tanúsítvány az tárolt hitelesítő adatok titkosításához és visszafejtéséhez használt.</span><span class="sxs-lookup"><span data-stu-id="a065b-380">However, as a best practice, ensure that both web and worker roles of your service deployments are kept up toodate and secure as they both have access toohello metadata database and hello certificate used for encryption and decryption of stored credentials.</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

