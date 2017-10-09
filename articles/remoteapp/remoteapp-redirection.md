---
title: "az Azure Remoteappban aaaUsing átirányítási |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconfigure és -felhasználási átirányítás a RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 2c8c867f-4907-4f2e-9ccd-2eb82bb5b837
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d5739a75cf606bd971268da67b2c5ff0fe5fe19b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-redirection-in-azure-remoteapp"></a><span data-ttu-id="08fb0-103">Az Azure Remoteappban átirányítással</span><span class="sxs-lookup"><span data-stu-id="08fb0-103">Using redirection in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="08fb0-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="08fb0-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="08fb0-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="08fb0-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="08fb0-106">Eszközátirányítás lehetővé teszi, hogy a felhasználók távoli alkalmazások hello eszközök csatolt tootheir helyi számítógépen, telefonját vagy táblagépét kommunikál.</span><span class="sxs-lookup"><span data-stu-id="08fb0-106">Device redirection lets your users interact with remote apps using hello devices attached tootheir local computer, phone, or tablet.</span></span> <span data-ttu-id="08fb0-107">Például hogy megadta az Azure RemoteApp Skype, a felhasználó számára szükséges hello kamera telepítve a számítógépen toowork a Skype.</span><span class="sxs-lookup"><span data-stu-id="08fb0-107">For example, if you have provided Skype through Azure RemoteApp, your user needs hello camera installed on their PC toowork with Skype.</span></span> <span data-ttu-id="08fb0-108">Ez akkor is igaz, nyomtatók, hangszórók, figyelők és a tartomány az USB-csatlakozóval csatlakoztatott perifériák.</span><span class="sxs-lookup"><span data-stu-id="08fb0-108">This is also true for printers, speakers, monitors, and a range of USB-connected peripherals.</span></span>

<span data-ttu-id="08fb0-109">RemoteApp hello Remote Desktop Protocol (RDP) és a RemoteFX tooprovide átirányítási kihasználja.</span><span class="sxs-lookup"><span data-stu-id="08fb0-109">RemoteApp leverages hello Remote Desktop Protocol (RDP) and RemoteFX tooprovide redirection.</span></span>

## <a name="what-redirection-is-enabled-by-default"></a><span data-ttu-id="08fb0-110">Milyen átirányítás alapértelmezés szerint engedélyezve van?</span><span class="sxs-lookup"><span data-stu-id="08fb0-110">What redirection is enabled by default?</span></span>
<span data-ttu-id="08fb0-111">RemoteApp használata esetén a következő átirányítások hello alapértelmezés szerint engedélyezve vannak.</span><span class="sxs-lookup"><span data-stu-id="08fb0-111">When you use RemoteApp, hello following redirections are enabled by default.</span></span> <span data-ttu-id="08fb0-112">hello információk zárójelben hello RDP-beállítás megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="08fb0-112">hello information in parentheses show hello RDP setting.</span></span>

* <span data-ttu-id="08fb0-113">Hang lejátszása hello helyi számítógépen (**ezen a számítógépen lejátszása**).</span><span class="sxs-lookup"><span data-stu-id="08fb0-113">Play sounds on hello local computer (**Play on this computer**).</span></span> <span data-ttu-id="08fb0-114">(audiomode:i:0)</span><span class="sxs-lookup"><span data-stu-id="08fb0-114">(audiomode:i:0)</span></span>
* <span data-ttu-id="08fb0-115">A hello helyi számítógép és a küldési toohello távoli számítógépről hang rögzítése (**erről a számítógépről rögzítse**).</span><span class="sxs-lookup"><span data-stu-id="08fb0-115">Capture audio from hello local computer and send toohello remote computer (**Record from this computer**).</span></span> <span data-ttu-id="08fb0-116">(audiocapturemode:i:1)</span><span class="sxs-lookup"><span data-stu-id="08fb0-116">(audiocapturemode:i:1)</span></span>
* <span data-ttu-id="08fb0-117">Nyomtatási toolocal nyomtatók (redirectprinters:i:1)</span><span class="sxs-lookup"><span data-stu-id="08fb0-117">Print toolocal printers (redirectprinters:i:1)</span></span>
* <span data-ttu-id="08fb0-118">COM-portokhoz (redirectcomports:i:1)</span><span class="sxs-lookup"><span data-stu-id="08fb0-118">COM ports (redirectcomports:i:1)</span></span>
* <span data-ttu-id="08fb0-119">Intelligens kártya eszköz (redirectsmartcards:i:1)</span><span class="sxs-lookup"><span data-stu-id="08fb0-119">Smart card device (redirectsmartcards:i:1)</span></span>
* <span data-ttu-id="08fb0-120">Vágólap (képességét toocopy és a Beillesztés) (redirectclipboard:i:1)</span><span class="sxs-lookup"><span data-stu-id="08fb0-120">Clipboard (ability toocopy and paste) (redirectclipboard:i:1)</span></span>
* <span data-ttu-id="08fb0-121">Törölje a típus betűsimítás (betűsimítás engedélyezése: i:1)</span><span class="sxs-lookup"><span data-stu-id="08fb0-121">Clear type font smoothing (allow font smoothing:i:1)</span></span>
* <span data-ttu-id="08fb0-122">Támogatott Plug and Play eszközök átirányítása.</span><span class="sxs-lookup"><span data-stu-id="08fb0-122">Redirect all supported Plug and Play devices.</span></span> <span data-ttu-id="08fb0-123">(devicestoredirect:s: *)</span><span class="sxs-lookup"><span data-stu-id="08fb0-123">(devicestoredirect:s:*)</span></span>

## <a name="what-other-redirection-is-available"></a><span data-ttu-id="08fb0-124">Milyen más átirányítási érhető el?</span><span class="sxs-lookup"><span data-stu-id="08fb0-124">What other redirection is available?</span></span>
<span data-ttu-id="08fb0-125">Két átirányítási alapesetben le vannak tiltva:</span><span class="sxs-lookup"><span data-stu-id="08fb0-125">Two redirection options are disabled by default:</span></span>

* <span data-ttu-id="08fb0-126">Átirányítás (meghajtó-hozzárendeléssel): A helyi számítógép meghajtók hello távoli munkamenetben csatlakoztatott meghajtók válnak.</span><span class="sxs-lookup"><span data-stu-id="08fb0-126">Drive redirection (drive mapping): Your local computer's drives become mapped drives in hello remote session.</span></span> <span data-ttu-id="08fb0-127">Ez lehetővé teszi, hogy menti vagy a helyi meghajtók a megnyitott fájlok munka hello távoli munkamenet közben.</span><span class="sxs-lookup"><span data-stu-id="08fb0-127">This lets you save or open files from your local drives while you work in hello remote session.</span></span>
* <span data-ttu-id="08fb0-128">USB-átirányítása: hello távoli munkamenetben hello USB eszközök csatolt tooyour helyi számítógépen is használhatja.</span><span class="sxs-lookup"><span data-stu-id="08fb0-128">USB redirection: You can use hello USB devices attached tooyour local computer within hello remote session.</span></span>

## <a name="change-your-redirection-settings-in-remoteapp"></a><span data-ttu-id="08fb0-129">A RemoteApp a mappaátirányítási beállítások módosítása</span><span class="sxs-lookup"><span data-stu-id="08fb0-129">Change your redirection settings in RemoteApp</span></span>
<span data-ttu-id="08fb0-130">SDK-val hello Microsoft Azure PowerShell használatával módosíthatja a hello eszköz Mappaátirányítási beállítások gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="08fb0-130">You can change hello device redirection settings for a collection by using hello Microsoft Azure PowerShell with SDK.</span></span> <span data-ttu-id="08fb0-131">Telepítése után új PowerShell és az SDK hello, először konfigurálja előfizetése kezeléséhez ismertetett toomanage [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="08fb0-131">After you install hello new PowerShell and SDK, first configure it toomanage your subscription as described in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="08fb0-132">Kövesse a következő tooset hello egyéni RDP tulajdonságai parancs hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="08fb0-132">Then use a command similar toohello following tooset hello custom RDP properties:</span></span>

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

<span data-ttu-id="08fb0-133">(Vegye figyelembe, hogy  *\`n*  használt elválasztó egyes tulajdonságai között.)</span><span class="sxs-lookup"><span data-stu-id="08fb0-133">(Note that *\`n* is used as a delimiter between individual properties.)</span></span>

<span data-ttu-id="08fb0-134">tooget mely egyéni RDP-tulajdonságok vannak konfigurálva, futtassa a következő parancsmag hello listáját.</span><span class="sxs-lookup"><span data-stu-id="08fb0-134">tooget a list of what custom RDP properties are configured, run hello following cmdlet.</span></span> <span data-ttu-id="08fb0-135">Vegye figyelembe, hogy csak egyéni tulajdonságok jelennek meg a kimeneti eredmények, és nem az alapértelmezett tulajdonságokat hello:</span><span class="sxs-lookup"><span data-stu-id="08fb0-135">Note that only custom properties are shown as output results and not hello default properties:</span></span>  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

<span data-ttu-id="08fb0-136">Ha úgy állítja be az egyéni tulajdonságokat meg kell adnia minden egyéni tulajdonságok minden alkalommal, amikor; Ellenkező esetben a hello beállítás toodisabled visszaállítja.</span><span class="sxs-lookup"><span data-stu-id="08fb0-136">When you set custom properties you must specify all custom properties each time; otherwise hello setting reverts toodisabled.</span></span>   

### <a name="common-examples"></a><span data-ttu-id="08fb0-137">Gyakori példák</span><span class="sxs-lookup"><span data-stu-id="08fb0-137">Common examples</span></span>
<span data-ttu-id="08fb0-138">A következő parancsmag tooenable átirányítás hello használata:</span><span class="sxs-lookup"><span data-stu-id="08fb0-138">Use hello following cmdlet tooenable drive redirection:</span></span>  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*"

<span data-ttu-id="08fb0-139">Ez a parancsmag tooenable használja USB és meghajtó átirányítás:</span><span class="sxs-lookup"><span data-stu-id="08fb0-139">Use this cmdlet tooenable both USB and Drive redirection:</span></span>

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

<span data-ttu-id="08fb0-140">Használja a parancsmag toodisable vágólap megosztása:</span><span class="sxs-lookup"><span data-stu-id="08fb0-140">Use this cmdlet toodisable clipboard sharing:</span></span>  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0"

> [!IMPORTANT]
> <span data-ttu-id="08fb0-141">Lehet, hogy toocompletely kijelentkezés hello gyűjteményben lévő összes felhasználók (és nem csak válassza le őket) hello módosítás tesztelése előtt.</span><span class="sxs-lookup"><span data-stu-id="08fb0-141">Be sure toocompletely log off all users in hello collection (and not just disconnect them) before you test hello change.</span></span> <span data-ttu-id="08fb0-142">tooensure felhasználók teljesen kilépéséig, nyissa meg toohello **munkamenetek** hello gyűjtemény hello Azure-portálon a lapra, és jelentkezzen ki minden leválasztása és bejelentkezett felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="08fb0-142">tooensure users are completely logged off, go toohello **Sessions** tab in hello collection in hello Azure portal and log off any users who are disconnected or signed in.</span></span> <span data-ttu-id="08fb0-143">Egyes esetekben is igénybe vehet pár másodpercig hello helyi meghajtók tooshow Explorer hello-munkameneten belül.</span><span class="sxs-lookup"><span data-stu-id="08fb0-143">Sometimes it can take several seconds for hello local drives tooshow in Explorer within hello session.</span></span>
> 
> 

## <a name="change-usb-redirection-settings-on-your-windows-client"></a><span data-ttu-id="08fb0-144">A Windows-ügyfeleken USB-átirányítása beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="08fb0-144">Change USB redirection settings on your Windows client</span></span>
<span data-ttu-id="08fb0-145">Ha azt szeretné, hogy a számítógépen, amely a tooRemoteApp toouse USB-átirányítása, nincsenek 2 toohappen szükséges műveleteket.</span><span class="sxs-lookup"><span data-stu-id="08fb0-145">If you want toouse USB redirection on a computer that connects tooRemoteApp, there are 2 actions that need toohappen.</span></span> <span data-ttu-id="08fb0-146">1 – a rendszergazdának, hello gyűjtemény szintjén tooenable USB-átirányítása Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="08fb0-146">1 - Your administrator needs tooenable USB redirection at hello collection level by using Azure PowerShell.</span></span> <span data-ttu-id="08fb0-147">2 - az egyes eszközökön, ha azt szeretné, hogy toouse USB-átirányítása tooenable olyan csoportházirenddel, amely lehetővé teszi, hogy szüksége.</span><span class="sxs-lookup"><span data-stu-id="08fb0-147">2 - On each device where you want toouse USB redirection, you need tooenable a group policy that permits it.</span></span> <span data-ttu-id="08fb0-148">Ezzel a lépéssel kell toobe minden olyan felhasználóhoz, szeretne toouse USB-átirányítása történik.</span><span class="sxs-lookup"><span data-stu-id="08fb0-148">This step will need toobe done for each user that wants toouse USB redirection.</span></span>

> [!NOTE]
> <span data-ttu-id="08fb0-149">Az Azure RemoteApp USB-átirányítása csak a Windows rendszerű számítógépeken támogatott.</span><span class="sxs-lookup"><span data-stu-id="08fb0-149">USB redirection with Azure RemoteApp is only supported for Windows computers.</span></span>
> 
> 

### <a name="enable-usb-redirection-for-hello-remoteapp-collection"></a><span data-ttu-id="08fb0-150">A RemoteApp-gyűjtemény hello USB-átirányítása engedélyezése</span><span class="sxs-lookup"><span data-stu-id="08fb0-150">Enable USB redirection for hello RemoteApp collection</span></span>
<span data-ttu-id="08fb0-151">A következő parancsmag tooenable USB-átirányítása hello gyűjtemény szintjén hello használata:</span><span class="sxs-lookup"><span data-stu-id="08fb0-151">Use hello following cmdlet tooenable USB redirection at hello collection level:</span></span>

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-hello-client-computer"></a><span data-ttu-id="08fb0-152">USB-átirányítása hello ügyfélszámítógép engedélyezése</span><span class="sxs-lookup"><span data-stu-id="08fb0-152">Enable USB redirection for hello client computer</span></span>
<span data-ttu-id="08fb0-153">tooconfigure USB Mappaátirányítási beállítások a számítógépen:</span><span class="sxs-lookup"><span data-stu-id="08fb0-153">tooconfigure USB redirection settings on your computer:</span></span>

1. <span data-ttu-id="08fb0-154">Nyissa meg a Helyicsoportházirend-szerkesztő (GPEDIT hello. MSC).</span><span class="sxs-lookup"><span data-stu-id="08fb0-154">Open hello Local Group Policy Editor (GPEDIT.MSC).</span></span> <span data-ttu-id="08fb0-155">(Gpedit.msc futtassa a parancssorba.)</span><span class="sxs-lookup"><span data-stu-id="08fb0-155">(Run gpedit.msc from a command prompt.)</span></span>
2. <span data-ttu-id="08fb0-156">Nyissa meg **számítógép Configuration\Policies\Administrative Templates\Windows összetevők\Távoli asztali szolgáltatások\Távoli asztali kapcsolat Client\RemoteFX USB eszközátirányítás**.</span><span class="sxs-lookup"><span data-stu-id="08fb0-156">Open **Computer Configuration\Policies\Administrative Templates\Windows Components\Remote Desktop Services\Remote Desktop Connection Client\RemoteFX USB Device Redirection**.</span></span>
3. <span data-ttu-id="08fb0-157">Kattintson duplán a **RDP engedélyezése átirányítása egyéb támogatott RemoteFX USB-eszközök erről a számítógépről**.</span><span class="sxs-lookup"><span data-stu-id="08fb0-157">Double-click **Allow RDP redirection of other supported RemoteFX USB devices from this computer**.</span></span>
4. <span data-ttu-id="08fb0-158">Válassza ki **engedélyezve**, majd válassza ki **hello RemoteFX USB-átirányítása hozzáférési jogok lévő Rendszergazdák és felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="08fb0-158">Select **Enabled**, and then select **Administrators and Users in hello RemoteFX USB Redirection Access Rights**.</span></span>
5. <span data-ttu-id="08fb0-159">Nyisson meg egy parancssort rendszergazdai engedélyekkel, és futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="08fb0-159">Open a command prompt with administrative permissions, and run hello following command:</span></span>
   
        gpupdate /force
6. <span data-ttu-id="08fb0-160">Indítsa újra a hello számítógépet.</span><span class="sxs-lookup"><span data-stu-id="08fb0-160">Restart hello computer.</span></span>

<span data-ttu-id="08fb0-161">Is használhat hello Csoportházirend kezelése eszköz toocreate és hello USB átirányítási házirend az összes számítógép alkalmazni a tartományban:</span><span class="sxs-lookup"><span data-stu-id="08fb0-161">You can also use hello Group Policy Management tool toocreate and apply hello USB redirection policy for all computers in your domain:</span></span>

1. <span data-ttu-id="08fb0-162">Jelentkezzen be a tartományvezérlő hello hello tartományi rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="08fb0-162">Log into hello domain controller as hello domain administrator.</span></span>
2. <span data-ttu-id="08fb0-163">Nyissa meg a Csoportházirend kezelése konzol hello.</span><span class="sxs-lookup"><span data-stu-id="08fb0-163">Open hello Group Policy Management Console.</span></span> <span data-ttu-id="08fb0-164">(Kattintson **Start > Felügyeleti eszközök > csoportházirend-kezelés**.)</span><span class="sxs-lookup"><span data-stu-id="08fb0-164">(Click **Start > Administrative Tools > Group Policy Management**.)</span></span>
3. <span data-ttu-id="08fb0-165">Keresse meg a toohello tartomány vagy szervezeti egység legyen toocreate hello házirend.</span><span class="sxs-lookup"><span data-stu-id="08fb0-165">Navigate toohello domain or organizational unit for which you want toocreate hello policy.</span></span>
4. <span data-ttu-id="08fb0-166">Kattintson a jobb gombbal **alapértelmezett tartományházirend**, és kattintson a **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="08fb0-166">Right-click **Default Domain Policy**, and then click **Edit**.</span></span>
5. <span data-ttu-id="08fb0-167">Nyissa meg **számítógép Configuration\Policies\Administrative Templates\Windows összetevők\Távoli asztali szolgáltatások\Távoli asztali kapcsolat Client\RemoteFX USB eszközátirányítás**.</span><span class="sxs-lookup"><span data-stu-id="08fb0-167">Open **Computer Configuration\Policies\Administrative Templates\Windows Components\Remote Desktop Services\Remote Desktop Connection Client\RemoteFX USB Device Redirection**.</span></span>
6. <span data-ttu-id="08fb0-168">Kattintson duplán a **RDP engedélyezése átirányítása egyéb támogatott RemoteFX USB-eszközök erről a számítógépről**.</span><span class="sxs-lookup"><span data-stu-id="08fb0-168">Double-click **Allow RDP redirection of other supported RemoteFX USB devices from this computer**.</span></span>
7. <span data-ttu-id="08fb0-169">Válassza ki **engedélyezve**, majd válassza ki **hello RemoteFX USB-átirányítása hozzáférési jogok lévő Rendszergazdák és felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="08fb0-169">Select **Enabled**, and then select **Administrators and Users in hello RemoteFX USB Redirection Access Rights**.</span></span>
8. <span data-ttu-id="08fb0-170">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="08fb0-170">Click **OK**.</span></span>  

