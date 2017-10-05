---
title: "Azure Cloud Services szerepkörök – gyakori kérdések |} Microsoft Docs"
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
ms.openlocfilehash: d887f3b31693c414254dc01dac4dbdd6d9224b6d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="cloud-services-faq"></a><span data-ttu-id="2760c-104">Cloud Services – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="2760c-104">Cloud Services FAQ</span></span>
<span data-ttu-id="2760c-105">Ebben a cikkben megválaszolunk néhány gyakori kérdés a Microsoft Azure felhőalapú szolgáltatásairól.</span><span class="sxs-lookup"><span data-stu-id="2760c-105">This article answers some frequently asked questions about Microsoft Azure Cloud Services.</span></span> <span data-ttu-id="2760c-106">Is letöltheti a [Azure támogatási gyakori Kérdéseinek](http://go.microsoft.com/fwlink/?LinkID=185083) általános Azure tarifa- és támogatási információkat.</span><span class="sxs-lookup"><span data-stu-id="2760c-106">You can also visit the [Azure Support FAQ](http://go.microsoft.com/fwlink/?LinkID=185083) for general Azure pricing and support information.</span></span> <span data-ttu-id="2760c-107">Is tud kezelni a [felhőalapú szolgáltatások Virtuálisgép-méretet lap](cloud-services-sizes-specs.md) mérete információt.</span><span class="sxs-lookup"><span data-stu-id="2760c-107">You can also consult the [Cloud Services VM Size page](cloud-services-sizes-specs.md) for size information.</span></span>

## <a name="certificates"></a><span data-ttu-id="2760c-108">Tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="2760c-108">Certificates</span></span>
### <a name="where-should-i-install-my-certificate"></a><span data-ttu-id="2760c-109">Hol kell telepíteni a tanúsítványt?</span><span class="sxs-lookup"><span data-stu-id="2760c-109">Where should I install my certificate?</span></span>
* <span data-ttu-id="2760c-110">**A**</span><span class="sxs-lookup"><span data-stu-id="2760c-110">**My**</span></span>  
  <span data-ttu-id="2760c-111">Alkalmazás titkos kulcsot tartalmazó tanúsítvány (\*.pfx, \*.p12).</span><span class="sxs-lookup"><span data-stu-id="2760c-111">Application Certificate with private key (\*.pfx, \*.p12).</span></span>
* <span data-ttu-id="2760c-112">**HITELESÍTÉSSZOLGÁLTATÓ**</span><span class="sxs-lookup"><span data-stu-id="2760c-112">**CA**</span></span>  
  <span data-ttu-id="2760c-113">Ezt a tárolót (házirend- és alárendelt hitelesítésszolgáltatókat) keresse meg a köztes tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="2760c-113">All your intermediate certificates go in this store (Policy and Sub CAs).</span></span>
* <span data-ttu-id="2760c-114">**LEGFELSŐ SZINTŰ**</span><span class="sxs-lookup"><span data-stu-id="2760c-114">**ROOT**</span></span>  
  <span data-ttu-id="2760c-115">A legfelső szintű hitelesítésszolgáltató tárolja, ezért itt kell nyissa meg a fő legfelső szintű Hitelesítésszolgáltatói tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="2760c-115">The root CA store, so your main root CA cert should go here.</span></span>

### <a name="i-cant-remove-expired-certificate"></a><span data-ttu-id="2760c-116">A lejárt tanúsítvány nem távolítható el</span><span class="sxs-lookup"><span data-stu-id="2760c-116">I can't remove expired certificate</span></span>
<span data-ttu-id="2760c-117">Azure megakadályozza a tanúsítvány eltávolítása, miközben használatban van.</span><span class="sxs-lookup"><span data-stu-id="2760c-117">Azure prevents you from removing a certificate while it is in use.</span></span> <span data-ttu-id="2760c-118">Vagy törölje a központi telepítést, a tanúsítványt használó, vagy frissítse a központi telepítés egy másik vagy megújított tanúsítványt kell.</span><span class="sxs-lookup"><span data-stu-id="2760c-118">You need to either delete the deployment that uses the certificate, or update the deployment with a different or renewed certificate.</span></span>

### <a name="delete-an-expired-certificate"></a><span data-ttu-id="2760c-119">A lejárt tanúsítvány törlése</span><span class="sxs-lookup"><span data-stu-id="2760c-119">Delete an expired certificate</span></span>
<span data-ttu-id="2760c-120">Mindaddig, amíg a tanúsítvány nincs használatban, használhatja a [Remove-AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell-parancsmag segítségével távolítsa el a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="2760c-120">As long as the certificate is not in use, you can use the [Remove-AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) PowerShell cmdlet to remove a certificate.</span></span>

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a><span data-ttu-id="2760c-121">I lejárt tanúsítványok nevű Windows Azure Szolgáltatáskezelés-bővítmények</span><span class="sxs-lookup"><span data-stu-id="2760c-121">I have expired certificates named Windows Azure Service Management for Extensions</span></span>
<span data-ttu-id="2760c-122">Ezek a tanúsítványok jönnek létre, amikor egy bővítmény hozzáadódik a felhőszolgáltatásra, például a távoli asztali bővítmény.</span><span class="sxs-lookup"><span data-stu-id="2760c-122">These certificates are created whenever an extension is added to the cloud service such as the Remote Desktop extension.</span></span> <span data-ttu-id="2760c-123">Ezek a tanúsítványok csak titkosítása és visszafejtése a bővítmény privát konfigurációjának használják.</span><span class="sxs-lookup"><span data-stu-id="2760c-123">These certificates are only used for encrypting and decrypting the private configuration of the extension.</span></span> <span data-ttu-id="2760c-124">Nem számít, ha ezek a tanúsítványok lejárnak.</span><span class="sxs-lookup"><span data-stu-id="2760c-124">It does not matter if these certificates expire.</span></span> <span data-ttu-id="2760c-125">A lejárati dátum a rendszer nem ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="2760c-125">The expiration date is not checked.</span></span>

### <a name="certificates-i-have-deleted-keep-reappearing"></a><span data-ttu-id="2760c-126">Törölt rendelkezik tanúsítványok továbbra is megjelennek</span><span class="sxs-lookup"><span data-stu-id="2760c-126">Certificates I have deleted keep reappearing</span></span>
<span data-ttu-id="2760c-127">Ezek továbbra is megjelennek a legvalószínűbb ok miatt használ, például a Visual Studio eszközt.</span><span class="sxs-lookup"><span data-stu-id="2760c-127">These keep reappearing most likely because of a tool you're using, such as Visual Studio.</span></span> <span data-ttu-id="2760c-128">Amikor újra kapcsolódik egy eszközt, amely olyan tanúsítványt használ, azt újra feltöltés Azure.</span><span class="sxs-lookup"><span data-stu-id="2760c-128">Whenever you reconnect with a tool that is using a certificate, it will again be uploaded to Azure.</span></span>

### <a name="my-certificates-keep-disappearing"></a><span data-ttu-id="2760c-129">A tanúsítványok ne hagyja tűnhessenek</span><span class="sxs-lookup"><span data-stu-id="2760c-129">My certificates keep disappearing</span></span>
<span data-ttu-id="2760c-130">A virtuálisgép-példányt újraindul, az összes helyi módosítások elvesznek.</span><span class="sxs-lookup"><span data-stu-id="2760c-130">When the virtual machine instance recycles, all local changes are lost.</span></span> <span data-ttu-id="2760c-131">Használja a [indítási tevékenységhez](cloud-services-startup-tasks.md) tanúsítványok telepítése a virtuális géphez a szerepkör minden egyes indításakor.</span><span class="sxs-lookup"><span data-stu-id="2760c-131">Use a [startup task](cloud-services-startup-tasks.md) to install certificates to the virtual machine each time the role starts.</span></span>

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a><span data-ttu-id="2760c-132">Nem található a felügyeleti tanúsítványok a portálon</span><span class="sxs-lookup"><span data-stu-id="2760c-132">I cannot find my management certificates in the portal</span></span>
<span data-ttu-id="2760c-133">[Felügyeleti tanúsítványok](../azure-api-management-certs.md) csak érhetők el a klasszikus Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2760c-133">[Management certificates](../azure-api-management-certs.md) are only available in the Azure Classic Portal.</span></span> <span data-ttu-id="2760c-134">Azure-portálon nem használja a felügyeleti tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="2760c-134">The current Azure portal does not use management certificates.</span></span> 

### <a name="how-can-i-disable-a-management-certificate"></a><span data-ttu-id="2760c-135">Hogyan letilthatja egy felügyeleti tanúsítványt?</span><span class="sxs-lookup"><span data-stu-id="2760c-135">How can I disable a management certificate?</span></span>
<span data-ttu-id="2760c-136">[Felügyeleti tanúsítványok](../azure-api-management-certs.md) nem tiltható le.</span><span class="sxs-lookup"><span data-stu-id="2760c-136">[Management certificates](../azure-api-management-certs.md) cannot be disabled.</span></span> <span data-ttu-id="2760c-137">Törli a klasszikus Azure portálon keresztül, ha azt szeretné, hogy már használható.</span><span class="sxs-lookup"><span data-stu-id="2760c-137">You delete them through the Azure Classic Portal when you do not want them to be used anymore.</span></span>

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a><span data-ttu-id="2760c-138">Hogyan hozható létre egy adott IP-cím SSL-tanúsítvány?</span><span class="sxs-lookup"><span data-stu-id="2760c-138">How do I create an SSL certificate for a specific IP address?</span></span>
<span data-ttu-id="2760c-139">Kövesse a riasztásban megjelenő utasításokat a [hozzon létre egy tanúsítványt az oktatóanyag](cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="2760c-139">Follow the directions in the [create a certificate tutorial](cloud-services-certs-create.md).</span></span> <span data-ttu-id="2760c-140">Az IP-címet használják a DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="2760c-140">Use the IP address as the DNS Name.</span></span>

## <a name="security"></a><span data-ttu-id="2760c-141">Biztonság</span><span class="sxs-lookup"><span data-stu-id="2760c-141">Security</span></span>
### <a name="disable-ssl-30"></a><span data-ttu-id="2760c-142">Tiltsa le a SSL 3.0</span><span class="sxs-lookup"><span data-stu-id="2760c-142">Disable SSL 3.0</span></span>
<span data-ttu-id="2760c-143">Tiltsa le a SSL 3.0 és TLS biztonsági használatához ebben a blogbejegyzésben amely dokumentált indítási feladat létrehozása: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/</span><span class="sxs-lookup"><span data-stu-id="2760c-143">To disable SSL 3.0 and use TLS security, create a startup task which is documented on this blog post: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/</span></span>

### <a name="add-nosniff-to-your-website"></a><span data-ttu-id="2760c-144">Adja hozzá **nosniff** a webhelyhez</span><span class="sxs-lookup"><span data-stu-id="2760c-144">Add **nosniff** to your website</span></span>
<span data-ttu-id="2760c-145">Megakadályozza a felhasználókat a MIME-típusok elemzés, adja hozzá a megfelelő értéket a *web.config* fájlt.</span><span class="sxs-lookup"><span data-stu-id="2760c-145">To prevent clients from sniffing the MIME types, add a setting in your *web.config* file.</span></span>

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

<span data-ttu-id="2760c-146">Azt is megteheti ezt az IIS-beállításként.</span><span class="sxs-lookup"><span data-stu-id="2760c-146">You can also add this as a setting in IIS.</span></span> <span data-ttu-id="2760c-147">Használja a következő parancsot a [közös indítási feladatok](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) cikk.</span><span class="sxs-lookup"><span data-stu-id="2760c-147">Use the following command with the [common startup tasks](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) article.</span></span>

```cmd
%windir%\system32\inetsrv\appcmd set config /section:httpProtocol /+customHeaders.[name='X-Content-Type-Options',value='nosniff']
```

### <a name="customize-iis-for-a-web-role"></a><span data-ttu-id="2760c-148">A webes szerepkör IIS testreszabása</span><span class="sxs-lookup"><span data-stu-id="2760c-148">Customize IIS for a web role</span></span>
<span data-ttu-id="2760c-149">Az IIS indítási parancsfájl használatát a [közös indítási feladatok](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) cikk.</span><span class="sxs-lookup"><span data-stu-id="2760c-149">Use the IIS startup script from the [common startup tasks](cloud-services-startup-tasks-common.md#configure-iis-startup-with-appcmdexe) article.</span></span>

## <a name="scaling"></a><span data-ttu-id="2760c-150">Méretezés</span><span class="sxs-lookup"><span data-stu-id="2760c-150">Scaling</span></span>
### <a name="i-cannot-scale-beyond-x-instances"></a><span data-ttu-id="2760c-151">I nem felüli X példányok</span><span class="sxs-lookup"><span data-stu-id="2760c-151">I cannot scale beyond X instances</span></span>
<span data-ttu-id="2760c-152">Az Azure-előfizetés használhatja magok száma van korlátozva.</span><span class="sxs-lookup"><span data-stu-id="2760c-152">Your Azure Subscription has a limit on the number of cores you can use.</span></span> <span data-ttu-id="2760c-153">Skálázás nem működik, ha az összes rendelkezésre álló magot használja.</span><span class="sxs-lookup"><span data-stu-id="2760c-153">Scaling will not work if you have used all the cores available.</span></span> <span data-ttu-id="2760c-154">Például ha egy vonatkozó felső korlátját: 100, ez azt jelenti, 100 A1 méretű virtuálisgép-példánya lehet a felhőalapú szolgáltatás, vagy 50 A2 méretű virtuálisgép-példánya.</span><span class="sxs-lookup"><span data-stu-id="2760c-154">For example, if you have a limit of 100 cores, this means you could have 100 A1 sized virtual machine instances for your cloud service, or 50 A2 sized virtual machine instances.</span></span>

## <a name="networking"></a><span data-ttu-id="2760c-155">Hálózat</span><span class="sxs-lookup"><span data-stu-id="2760c-155">Networking</span></span>
### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a><span data-ttu-id="2760c-156">I multi-VIP felhőszolgáltatásban olyan IP-címet nem sikerült lefoglalni</span><span class="sxs-lookup"><span data-stu-id="2760c-156">I can't reserve an IP in a multi-VIP cloud service</span></span>
<span data-ttu-id="2760c-157">Először is győződjön meg arról, hogy a virtuálisgép-példányt, amely az IP-Címek lefoglalása próbál be van-e kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="2760c-157">First, make sure that the virtual machine instance that you're trying to reserve the IP for is turned on.</span></span> <span data-ttu-id="2760c-158">Második győződjön meg arról, hogy használata fenntartott IP-címek többet jelzi az átmeneti és üzemi központi telepítéseket.</span><span class="sxs-lookup"><span data-stu-id="2760c-158">Second, make sure that you're using Reserved IPs for bother the staging and production deployments.</span></span> <span data-ttu-id="2760c-159">**Ne** módosíthatja a beállításokat, amíg a telepítés frissítését végzi.</span><span class="sxs-lookup"><span data-stu-id="2760c-159">**Do not** change the settings while the deployment is upgrading.</span></span>

## <a name="remote-desktop"></a><span data-ttu-id="2760c-160">A távoli asztal</span><span class="sxs-lookup"><span data-stu-id="2760c-160">Remote desktop</span></span>
### <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a><span data-ttu-id="2760c-161">Hogyan készíthetek távoli asztal Ha egy NSG-t?</span><span class="sxs-lookup"><span data-stu-id="2760c-161">How do I remote desktop when I have an NSG?</span></span>
<span data-ttu-id="2760c-162">Szabályok hozzáadása az NSG-t, amelyek lehetővé teszik a forgalom a portokon **3389-es** és **20000**.</span><span class="sxs-lookup"><span data-stu-id="2760c-162">Add rules to the NSG that allow traffic on ports **3389** and **20000**.</span></span>  <span data-ttu-id="2760c-163">A távoli asztal-portot használja **3389-es**.</span><span class="sxs-lookup"><span data-stu-id="2760c-163">Remote Desktop uses port **3389**.</span></span>  <span data-ttu-id="2760c-164">Felhőalapú szolgáltatás példányainak kerül, így nem közvetlenül szabályozhatja, hogy melyik példányt való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="2760c-164">Cloud Service instances are load balanced, so you can't directly control which instance to connect to.</span></span>  <span data-ttu-id="2760c-165">A *RemoteForwarder* és *RemoteAccess* ügynökök kezelése RDP-forgalmát, és az ügyfél küldése az RDP cookie, és adjon meg egy egyedi példány való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="2760c-165">The *RemoteForwarder* and *RemoteAccess* agents manage RDP traffic and allow the client to send an RDP cookie and specify an individual instance to connect to.</span></span>  <span data-ttu-id="2760c-166">A *RemoteForwarder* és *RemoteAccess* ügynökök szükséges, hogy a port **20000*** nyitható meg, amely blokkolhatja, ha egy NSG.</span><span class="sxs-lookup"><span data-stu-id="2760c-166">The *RemoteForwarder* and *RemoteAccess* agents require that port **20000*** be opened, which may be blocked if you have an NSG.</span></span>
