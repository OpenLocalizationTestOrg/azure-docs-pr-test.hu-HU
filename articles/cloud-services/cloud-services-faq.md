---
title: "aaaAzure Felhőszolgáltatások szerepkörök – gyakori kérdések |} Microsoft Docs"
description: "Azure Cloud Services vonatkozó gyakran ismételt kérdések. Tanúsítványok, a webes szerepkörök és a feldolgozói szerepkörök kapcsolatos gyakori kérdésekre ad választ."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: b07a1990e031e60ae919a5f7c636945b89c7d3a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cloud-services-faq"></a><span data-ttu-id="b97e4-104">Cloud Services – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="b97e4-104">Cloud Services FAQ</span></span>
<span data-ttu-id="b97e4-105">Ebben a cikkben megválaszolunk néhány gyakori kérdés a Microsoft Azure felhőalapú szolgáltatásairól.</span><span class="sxs-lookup"><span data-stu-id="b97e4-105">This article answers some frequently asked questions about Microsoft Azure Cloud Services.</span></span> <span data-ttu-id="b97e4-106">Meglátogathatja hello [Azure támogatási gyakori Kérdéseinek](http://go.microsoft.com/fwlink/?LinkID=185083) általános Azure tarifa- és támogatási információkat.</span><span class="sxs-lookup"><span data-stu-id="b97e4-106">You can also visit hello [Azure Support FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) for general Azure pricing and support information.</span></span> <span data-ttu-id="b97e4-107">Is további részleteket hello [felhőalapú szolgáltatások Virtuálisgép-méretet lap](cloud-services-sizes-specs.md) mérete információt.</span><span class="sxs-lookup"><span data-stu-id="b97e4-107">You can also consult hello [Cloud Services VM Size page](cloud-services-sizes-specs.md) for size information.</span></span>

## <a name="certificates"></a><span data-ttu-id="b97e4-108">Tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="b97e4-108">Certificates</span></span>
### <a name="where-should-i-install-my-certificate"></a><span data-ttu-id="b97e4-109">Hol kell telepíteni a tanúsítványt?</span><span class="sxs-lookup"><span data-stu-id="b97e4-109">Where should I install my certificate?</span></span>
* <span data-ttu-id="b97e4-110">**A**</span><span class="sxs-lookup"><span data-stu-id="b97e4-110">**My**</span></span>  
  <span data-ttu-id="b97e4-111">Alkalmazás titkos kulcsot tartalmazó tanúsítvány (\*.pfx, \*.p12).</span><span class="sxs-lookup"><span data-stu-id="b97e4-111">Application Certificate with private key (\*.pfx, \*.p12).</span></span>
* <span data-ttu-id="b97e4-112">**HITELESÍTÉSSZOLGÁLTATÓ**</span><span class="sxs-lookup"><span data-stu-id="b97e4-112">**CA**</span></span>  
  <span data-ttu-id="b97e4-113">Ezt a tárolót (házirend- és alárendelt hitelesítésszolgáltatókat) keresse meg a köztes tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="b97e4-113">All your intermediate certificates go in this store (Policy and Sub CAs).</span></span>
* <span data-ttu-id="b97e4-114">**LEGFELSŐ SZINTŰ**</span><span class="sxs-lookup"><span data-stu-id="b97e4-114">**ROOT**</span></span>  
  <span data-ttu-id="b97e4-115">hello legfelső szintű hitelesítésszolgáltató tárolásához, ezért itt kell nyissa meg a fő legfelső szintű Hitelesítésszolgáltatói tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="b97e4-115">hello root CA store, so your main root CA cert should go here.</span></span>

### <a name="i-cant-remove-expired-certificate"></a><span data-ttu-id="b97e4-116">A lejárt tanúsítvány nem távolítható el</span><span class="sxs-lookup"><span data-stu-id="b97e4-116">I can't remove expired certificate</span></span>
<span data-ttu-id="b97e4-117">Azure megakadályozza a tanúsítvány eltávolítása, miközben használatban van.</span><span class="sxs-lookup"><span data-stu-id="b97e4-117">Azure prevents you from removing a certificate while it is in use.</span></span> <span data-ttu-id="b97e4-118">Tooeither delete hello telepítés hello tanúsítványt használ, vagy különböző vagy megújított tanúsítványt hello központi telepítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="b97e4-118">You need tooeither delete hello deployment that uses hello certificate, or update hello deployment with a different or renewed certificate.</span></span>

### <a name="delete-an-expired-certificate"></a><span data-ttu-id="b97e4-119">A lejárt tanúsítvány törlése</span><span class="sxs-lookup"><span data-stu-id="b97e4-119">Delete an expired certificate</span></span>
<span data-ttu-id="b97e4-120">Mindaddig, amíg hello tanúsítvány nincs használatban, használhatja a hello [Remove-AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell parancsmag tooremove egy tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="b97e4-120">As long as hello certificate is not in use, you can use hello [Remove-AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell cmdlet tooremove a certificate.</span></span>

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a><span data-ttu-id="b97e4-121">I lejárt tanúsítványok nevű Windows Azure Szolgáltatáskezelés-bővítmények</span><span class="sxs-lookup"><span data-stu-id="b97e4-121">I have expired certificates named Windows Azure Service Management for Extensions</span></span>
<span data-ttu-id="b97e4-122">Ezeket a tanúsítványokat jönnek létre, amikor egy bővítmény toohello felhőszolgáltatásra, például a távoli asztali bővítmény hello kerül.</span><span class="sxs-lookup"><span data-stu-id="b97e4-122">These certificates are created whenever an extension is added toohello cloud service such as hello Remote Desktop extension.</span></span> <span data-ttu-id="b97e4-123">Ezek a tanúsítványok csak titkosítása és visszafejtése hello a hello bővítmény privát konfigurációjának használják.</span><span class="sxs-lookup"><span data-stu-id="b97e4-123">These certificates are only used for encrypting and decrypting hello private configuration of hello extension.</span></span> <span data-ttu-id="b97e4-124">Nem számít, ha ezek a tanúsítványok lejárnak.</span><span class="sxs-lookup"><span data-stu-id="b97e4-124">It does not matter if these certificates expire.</span></span> <span data-ttu-id="b97e4-125">hello lejárati dátum a rendszer nem ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="b97e4-125">hello expiration date is not checked.</span></span>

### <a name="certificates-i-have-deleted-keep-reappearing"></a><span data-ttu-id="b97e4-126">Törölt rendelkezik tanúsítványok továbbra is megjelennek</span><span class="sxs-lookup"><span data-stu-id="b97e4-126">Certificates I have deleted keep reappearing</span></span>
<span data-ttu-id="b97e4-127">Ezek továbbra is megjelennek a legvalószínűbb ok miatt használ, például a Visual Studio eszközt.</span><span class="sxs-lookup"><span data-stu-id="b97e4-127">These keep reappearing most likely because of a tool you're using, such as Visual Studio.</span></span> <span data-ttu-id="b97e4-128">Csatlakozzon újra az olyan eszköz, amely olyan tanúsítványt használ, amikor újra lesz feltöltött tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b97e4-128">Whenever you reconnect with a tool that is using a certificate, it will again be uploaded tooAzure.</span></span>

### <a name="my-certificates-keep-disappearing"></a><span data-ttu-id="b97e4-129">A tanúsítványok ne hagyja tűnhessenek</span><span class="sxs-lookup"><span data-stu-id="b97e4-129">My certificates keep disappearing</span></span>
<span data-ttu-id="b97e4-130">Amikor virtuálisgép-példányt hello újraindul, az összes helyi módosítások elvesznek.</span><span class="sxs-lookup"><span data-stu-id="b97e4-130">When hello virtual machine instance recycles, all local changes are lost.</span></span> <span data-ttu-id="b97e4-131">Használja a [indítási tevékenységhez](cloud-services-startup-tasks.md) tooinstall tanúsítványok toohello virtuális gép minden alkalommal hello szerepkör elindul.</span><span class="sxs-lookup"><span data-stu-id="b97e4-131">Use a [startup task](cloud-services-startup-tasks.md) tooinstall certificates toohello virtual machine each time hello role starts.</span></span>

### <a name="i-cannot-find-my-management-certificates-in-hello-portal"></a><span data-ttu-id="b97e4-132">Nem található a felügyeleti tanúsítványok hello portálon</span><span class="sxs-lookup"><span data-stu-id="b97e4-132">I cannot find my management certificates in hello portal</span></span>
<span data-ttu-id="b97e4-133">[Felügyeleti tanúsítványok](../azure-api-management-certs.md) csak találhatók hello klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="b97e4-133">[Management certificates](../azure-api-management-certs.md) are only available in hello Azure Classic Portal.</span></span> <span data-ttu-id="b97e4-134">hello Azure-portálon nem használja a felügyeleti tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="b97e4-134">hello current Azure portal does not use management certificates.</span></span> 

### <a name="how-can-i-disable-a-management-certificate"></a><span data-ttu-id="b97e4-135">Hogyan letilthatja egy felügyeleti tanúsítványt?</span><span class="sxs-lookup"><span data-stu-id="b97e4-135">How can I disable a management certificate?</span></span>
<span data-ttu-id="b97e4-136">[Felügyeleti tanúsítványok](../azure-api-management-certs.md) nem tiltható le.</span><span class="sxs-lookup"><span data-stu-id="b97e4-136">[Management certificates](../azure-api-management-certs.md) cannot be disabled.</span></span> <span data-ttu-id="b97e4-137">Törli őket hello klasszikus Azure portálon keresztül, ha nem szeretné, hogy azok többé használt toobe.</span><span class="sxs-lookup"><span data-stu-id="b97e4-137">You delete them through hello Azure Classic Portal when you do not want them toobe used anymore.</span></span>

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a><span data-ttu-id="b97e4-138">Hogyan hozható létre egy adott IP-cím SSL-tanúsítvány?</span><span class="sxs-lookup"><span data-stu-id="b97e4-138">How do I create an SSL certificate for a specific IP address?</span></span>
<span data-ttu-id="b97e4-139">Hello hello útmutatásait követve [hozzon létre egy tanúsítványt az oktatóanyag](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="b97e4-139">Follow hello directions in hello [create a certificate tutorial](cloud-services-certs-create.md).</span></span> <span data-ttu-id="b97e4-140">DNS-név hello hello IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="b97e4-140">Use hello IP address as hello DNS Name.</span></span>

## <a name="security"></a><span data-ttu-id="b97e4-141">Biztonság</span><span class="sxs-lookup"><span data-stu-id="b97e4-141">Security</span></span>
### <a name="disable-ssl-30"></a><span data-ttu-id="b97e4-142">Tiltsa le a SSL 3.0</span><span class="sxs-lookup"><span data-stu-id="b97e4-142">Disable SSL 3.0</span></span>
<span data-ttu-id="b97e4-143">toodisable SSL 3.0 és TLS biztonság használata, amely dokumentálja indítási feladat létrehozása az ebben a blogbejegyzésben: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/</span><span class="sxs-lookup"><span data-stu-id="b97e4-143">toodisable SSL 3.0 and use TLS security, create a startup task which is documented on this blog post: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/</span></span>

### <a name="add-nosniff-tooyour-website"></a><span data-ttu-id="b97e4-144">Adja hozzá **nosniff** tooyour webhely</span><span class="sxs-lookup"><span data-stu-id="b97e4-144">Add **nosniff** tooyour website</span></span>
<span data-ttu-id="b97e4-145">elemzés hello MIME-típusok, az ügynökök tooprevent adja hozzá a megfelelő értéket a *web.config* fájlt.</span><span class="sxs-lookup"><span data-stu-id="b97e4-145">tooprevent clients from sniffing hello MIME types, add a setting in your *web.config* file.</span></span>

```xml
<configuration>
   <system.webServer>
      <httpProtocol>
         <customHeaders>
            <add name="X-Content-Type-Options" value="nosniff" />
         </customHeaders>
      </httpProtocol>
   </system.webServer>
</configuration>
```

<span data-ttu-id="b97e4-146">Azt is megteheti ezt az IIS-beállításként.</span><span class="sxs-lookup"><span data-stu-id="b97e4-146">You can also add this as a setting in IIS.</span></span> <span data-ttu-id="b97e4-147">Használjon hello következő parancsot a hello [közös indítási feladatok](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) cikk.</span><span class="sxs-lookup"><span data-stu-id="b97e4-147">Use hello following command with hello [common startup tasks](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) article.</span></span>

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="customize-iis-for-a-web-role"></a><span data-ttu-id="b97e4-148">A webes szerepkör IIS testreszabása</span><span class="sxs-lookup"><span data-stu-id="b97e4-148">Customize IIS for a web role</span></span>
<span data-ttu-id="b97e4-149">Hello IIS indítási parancsfájl hello használatát [közös indítási feladatok](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) cikk.</span><span class="sxs-lookup"><span data-stu-id="b97e4-149">Use hello IIS startup script from hello [common startup tasks](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) article.</span></span>

## <a name="scaling"></a><span data-ttu-id="b97e4-150">Méretezés</span><span class="sxs-lookup"><span data-stu-id="b97e4-150">Scaling</span></span>
### <a name="i-cannot-scale-beyond-x-instances"></a><span data-ttu-id="b97e4-151">I nem felüli X példányok</span><span class="sxs-lookup"><span data-stu-id="b97e4-151">I cannot scale beyond X instances</span></span>
<span data-ttu-id="b97e4-152">Az Azure-előfizetés használhatja magok hello számára van korlátozva.</span><span class="sxs-lookup"><span data-stu-id="b97e4-152">Your Azure Subscription has a limit on hello number of cores you can use.</span></span> <span data-ttu-id="b97e4-153">Skálázás nem működik, ha elérhető hello magot használja.</span><span class="sxs-lookup"><span data-stu-id="b97e4-153">Scaling will not work if you have used all hello cores available.</span></span> <span data-ttu-id="b97e4-154">Például ha egy vonatkozó felső korlátját: 100, ez azt jelenti, 100 A1 méretű virtuálisgép-példánya lehet a felhőalapú szolgáltatás, vagy 50 A2 méretű virtuálisgép-példánya.</span><span class="sxs-lookup"><span data-stu-id="b97e4-154">For example, if you have a limit of 100 cores, this means you could have 100 A1 sized virtual machine instances for your cloud service, or 50 A2 sized virtual machine instances.</span></span>

## <a name="networking"></a><span data-ttu-id="b97e4-155">Hálózat</span><span class="sxs-lookup"><span data-stu-id="b97e4-155">Networking</span></span>
### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a><span data-ttu-id="b97e4-156">I multi-VIP felhőszolgáltatásban olyan IP-címet nem sikerült lefoglalni</span><span class="sxs-lookup"><span data-stu-id="b97e4-156">I can't reserve an IP in a multi-VIP cloud service</span></span>
<span data-ttu-id="b97e4-157">Először is győződjön meg arról, hogy hello virtuálisgép-példányt próbált tooreserve hello IP-Címek be van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="b97e4-157">First, make sure that hello virtual machine instance that you're trying tooreserve hello IP for is turned on.</span></span> <span data-ttu-id="b97e4-158">Ezután ellenőrizze, hogy hello átmeneti és üzemi telepítések többet jelzi a foglalt IP-cím használja.</span><span class="sxs-lookup"><span data-stu-id="b97e4-158">Second, make sure that you're using Reserved IPs for bother hello staging and production deployments.</span></span> <span data-ttu-id="b97e4-159">**Ne** hello beállításainak módosítása közben hello telepítési frissítését végzi.</span><span class="sxs-lookup"><span data-stu-id="b97e4-159">**Do not** change hello settings while hello deployment is upgrading.</span></span>

## <a name="remote-desktop"></a><span data-ttu-id="b97e4-160">A távoli asztal</span><span class="sxs-lookup"><span data-stu-id="b97e4-160">Remote desktop</span></span>
### <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a><span data-ttu-id="b97e4-161">Hogyan készíthetek távoli asztal Ha egy NSG-t?</span><span class="sxs-lookup"><span data-stu-id="b97e4-161">How do I remote desktop when I have an NSG?</span></span>
<span data-ttu-id="b97e4-162">Adja hozzá a szabályok toohello, amelyek lehetővé teszik a forgalom a portokon NSG **3389-es** és **20000**.</span><span class="sxs-lookup"><span data-stu-id="b97e4-162">Add rules toohello NSG that allow traffic on ports **3389** and **20000**.</span></span>  <span data-ttu-id="b97e4-163">A távoli asztal-portot használja **3389-es**.</span><span class="sxs-lookup"><span data-stu-id="b97e4-163">Remote Desktop uses port **3389**.</span></span>  <span data-ttu-id="b97e4-164">Felhőalapú szolgáltatás példányainak kerül, így nem közvetlenül szabályozhatja, hogy melyik példányt tooconnect számára.</span><span class="sxs-lookup"><span data-stu-id="b97e4-164">Cloud Service instances are load balanced, so you can't directly control which instance tooconnect to.</span></span>  <span data-ttu-id="b97e4-165">Hello *RemoteForwarder* és *RemoteAccess* ügynökök RDP-forgalom kezelésére és hello ügyfél toosend RDP cookie-k engedélyezése, és adjon meg egy egyedi példány tooconnect való.</span><span class="sxs-lookup"><span data-stu-id="b97e4-165">hello *RemoteForwarder* and *RemoteAccess* agents manage RDP traffic and allow hello client toosend an RDP cookie and specify an individual instance tooconnect to.</span></span>  <span data-ttu-id="b97e4-166">Hello *RemoteForwarder* és *RemoteAccess* ügynökök szükséges, hogy a port **20000*** nyitható meg, amely blokkolhatja, ha egy NSG.</span><span class="sxs-lookup"><span data-stu-id="b97e4-166">hello *RemoteForwarder* and *RemoteAccess* agents require that port **20000*** be opened, which may be blocked if you have an NSG.</span></span>
