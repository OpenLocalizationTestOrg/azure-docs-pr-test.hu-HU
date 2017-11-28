---
title: "Az Azure Active Directory Connect: Gyakori kérdések – |} Microsoft Docs"
description: "Ezen a lapon az Azure AD Connect vonatkozó gyakran ismételt kérdések."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: fd5988b2d4170166902bb5cc39603d4a0f83be59
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a><span data-ttu-id="64445-103">Gyakori kérdések az Azure Active Directory Connect</span><span class="sxs-lookup"><span data-stu-id="64445-103">Frequently asked questions for Azure Active Directory Connect</span></span>

## <a name="general-installation"></a><span data-ttu-id="64445-104">Általános telepítési</span><span class="sxs-lookup"><span data-stu-id="64445-104">General installation</span></span>
<span data-ttu-id="64445-105">**K: telepítési működnek, ha az Azure AD globális rendszergazda 2FA engedélyezve van?**</span><span class="sxs-lookup"><span data-stu-id="64445-105">**Q: Will installation work if the Azure AD Global Admin has 2FA enabled?**</span></span>  
<span data-ttu-id="64445-106">A 2016. februári a buildek Ez támogatott.</span><span class="sxs-lookup"><span data-stu-id="64445-106">With the builds from February 2016, this is supported.</span></span>

<span data-ttu-id="64445-107">**K: az Azure AD Connect felügyelet nélküli telepítéséhez úgy van?**</span><span class="sxs-lookup"><span data-stu-id="64445-107">**Q: Is there a way to install Azure AD Connect unattended?**</span></span>  
<span data-ttu-id="64445-108">Csak a telepítési varázsló segítségével az Azure AD Connect telepítése támogatott.</span><span class="sxs-lookup"><span data-stu-id="64445-108">It is only supported to install Azure AD Connect using the installation wizard.</span></span> <span data-ttu-id="64445-109">Felügyelet nélküli és a csendes telepítés nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="64445-109">An unattended and silent installation is not supported.</span></span>

<span data-ttu-id="64445-110">**K: erdővel rendelkezem ahol tartománya nem érhető el. Hogyan kell telepíteni az Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="64445-110">**Q: I have a forest where one domain cannot be contacted. How do I install Azure AD Connect?**</span></span>  
<span data-ttu-id="64445-111">A 2016. februári a buildek Ez támogatott.</span><span class="sxs-lookup"><span data-stu-id="64445-111">With the builds from February 2016, this is supported.</span></span>

<span data-ttu-id="64445-112">**K: az Active Directory tartományi szolgáltatások health-ügynök működik a server core?**</span><span class="sxs-lookup"><span data-stu-id="64445-112">**Q: Does the AD DS health agent work on server core?**</span></span>  
<span data-ttu-id="64445-113">Igen.</span><span class="sxs-lookup"><span data-stu-id="64445-113">Yes.</span></span> <span data-ttu-id="64445-114">Az ügynök telepítése után végezze el a regisztrációs folyamat során a következő PowerShell-parancsmag használatával:</span><span class="sxs-lookup"><span data-stu-id="64445-114">After installing the agent, you can complete the registration process using the following PowerShell cmdlet:</span></span> 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a><span data-ttu-id="64445-115">Network (Hálózat)</span><span class="sxs-lookup"><span data-stu-id="64445-115">Network</span></span>
<span data-ttu-id="64445-116">**K: van egy tűzfal, a hálózati eszköz, vagy valami mást, amely korlátozza a maximális időt kapcsolatok maradhat, nyissa meg a hálózaton. Mennyi ideig kell a ügyfél ügyféloldali időkorlát küszöbértéke lehet, ha az Azure AD Connect használatával?**</span><span class="sxs-lookup"><span data-stu-id="64445-116">**Q: I have a firewall, network device, or something else that limits the maximum time connections can stay open on my network. How long should my client side timeout threshold be when using Azure AD Connect?**</span></span>  
<span data-ttu-id="64445-117">Minden hálózati szoftver, a fizikai eszközök vagy bármi más, amely korlátozza a maximális időt kapcsolatok nyitva maradhat a használata ajánlott a küszöbérték legalább 5 perc (300 másodperc) a kiszolgálóra, ahol az Azure AD Connect-ügyfél telepítve van és az Azure Active Directory közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="64445-117">All networking software, physical devices, or anything else that limits the maximum time connections can remain open should use a threshold of at least 5 minutes (300 seconds) for connectivity between the server where the Azure AD Connect client is installed and Azure Active Directory.</span></span> <span data-ttu-id="64445-118">Ez vonatkozik az összes korábban kiadott Microsoft Identity szinkronizálási eszközöket is.</span><span class="sxs-lookup"><span data-stu-id="64445-118">This also applies to all previously released Microsoft Identity synchronization tools.</span></span>

<span data-ttu-id="64445-119">**K: vannak (egy címke tartományok) által támogatott?**</span><span class="sxs-lookup"><span data-stu-id="64445-119">**Q: Are SLDs (Single Label Domains) supported?**</span></span>  
<span data-ttu-id="64445-120">Nem, az Azure AD Connect nem támogatja a helyi erdők/tartományok által használatával.</span><span class="sxs-lookup"><span data-stu-id="64445-120">No, Azure AD Connect does not support on-premises forests/domains using SLDs.</span></span>

<span data-ttu-id="64445-121">**K: vannak "pontozott" nevű NetBios támogatott?**</span><span class="sxs-lookup"><span data-stu-id="64445-121">**Q: Are "dotted" NetBios named supported?**</span></span>  
<span data-ttu-id="64445-122">Nem, az Azure AD Connect nem támogatja a helyi erdők/tartományok ahol a NetBIOS-név pontot tartalmaz "." a neve.</span><span class="sxs-lookup"><span data-stu-id="64445-122">No, Azure AD Connect does not support on-premises forests/domains where the NetBios name contains a period "." in the name.</span></span>

## <a name="federation"></a><span data-ttu-id="64445-123">Összevonási</span><span class="sxs-lookup"><span data-stu-id="64445-123">Federation</span></span>
<span data-ttu-id="64445-124">**K: Mit tehetek, ha jelenik meg egy e-mailt, amely az Office 365 tanúsítvány megújítása kérése**</span><span class="sxs-lookup"><span data-stu-id="64445-124">**Q: What do I do if I receive an email that asking me to renew my Office 365 certificate**</span></span>  
<span data-ttu-id="64445-125">Útmutatás a a [megújítani a tanúsítványokat](active-directory-aadconnect-o365-certs.md) témakör: a tanúsítvány megújításához.</span><span class="sxs-lookup"><span data-stu-id="64445-125">Use the guidance that is outlined in the [renew certificates](active-directory-aadconnect-o365-certs.md) topic on how to renew the certificate.</span></span>

<span data-ttu-id="64445-126">**K: van "automatikus frissítése függő" állítsa be az Office 365 függő entitáshoz. Kell tennie semmit, amikor a jogkivonat-aláíró tanúsítványa automatikusan visszaállítja a?**</span><span class="sxs-lookup"><span data-stu-id="64445-126">**Q: I have "Automatically update relying party" set for O365 relying party. Do I have to take any action when my token signing certificate automatically rolls over?**</span></span>  
<span data-ttu-id="64445-127">A cikkben ismertetett útmutatás [megújítani a tanúsítványokat](active-directory-aadconnect-o365-certs.md).</span><span class="sxs-lookup"><span data-stu-id="64445-127">Use the guidance that is outlined in the article [renew certificates](active-directory-aadconnect-o365-certs.md).</span></span>

## <a name="environment"></a><span data-ttu-id="64445-128">Környezet</span><span class="sxs-lookup"><span data-stu-id="64445-128">Environment</span></span>
<span data-ttu-id="64445-129">**K: támogatott a kiszolgáló átnevezése, az Azure AD Connect telepítése után?**</span><span class="sxs-lookup"><span data-stu-id="64445-129">**Q: Is it supported to rename the server after Azure AD Connect has been installed?**</span></span>  
<span data-ttu-id="64445-130">Nem.</span><span class="sxs-lookup"><span data-stu-id="64445-130">No.</span></span> <span data-ttu-id="64445-131">A kiszolgálónév módosítása esetén a szinkronizálási motor nem lehet kapcsolódni az SQL-adatbázishoz, és a szolgáltatás nem fogja tudni elindítani.</span><span class="sxs-lookup"><span data-stu-id="64445-131">Changing the server name will cause the sync engine to not be able to connect to the SQL database and the service will not be able to start.</span></span>

## <a name="identity-data"></a><span data-ttu-id="64445-132">Azonosító adatok</span><span class="sxs-lookup"><span data-stu-id="64445-132">Identity data</span></span>
<span data-ttu-id="64445-133">**K: az egyszerű felhasználónév (userPrincipalName) attribútum az Azure ad-ben nem egyezik a helyszíni UPN - miért?**</span><span class="sxs-lookup"><span data-stu-id="64445-133">**Q: The UPN (userPrincipalName) attribute in Azure AD does not match the on-prem UPN - why?**</span></span>  
<span data-ttu-id="64445-134">Ezek a cikkek lásd:</span><span class="sxs-lookup"><span data-stu-id="64445-134">See these articles:</span></span>

* [<span data-ttu-id="64445-135">Office 365, az Azure vagy az Intune-ban szereplő felhasználónevek nem egyeznek meg, a helyszíni egyszerű Felhasználónévvel vagy másodlagos bejelentkezési Azonosítóval</span><span class="sxs-lookup"><span data-stu-id="64445-135">User names in Office 365, Azure, or Intune don't match the on-premises UPN or alternate login ID</span></span>](https://support.microsoft.com/en-us/kb/2523192)
* [<span data-ttu-id="64445-136">A felhasználói fiók egyszerű másik összevont tartományt használandó módosítása után a módosítások nem az Azure Active Directory szinkronizáló eszköz által szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="64445-136">Changes aren't synced by the Azure Active Directory Sync tool after you change the UPN of a user account to use a different federated domain</span></span>](https://support.microsoft.com/en-us/kb/2669550)

<span data-ttu-id="64445-137">Beállíthatja úgy is engedélyezi a szinkronizálási motor frissítése a userPrincipalName leírtak szerint az Azure AD [az Azure AD Connect szinkronizálási szolgáltatás szolgáltatások](active-directory-aadconnectsyncservice-features.md).</span><span class="sxs-lookup"><span data-stu-id="64445-137">You can also configure Azure AD to allow the sync engine to update the userPrincipalName as described in [Azure AD Connect sync service features](active-directory-aadconnectsyncservice-features.md).</span></span>

<span data-ttu-id="64445-138">**K: enyhe találatra támogatja azt a helyszíni AD-csoport vagy partner meglévő Azure AD-csoport vagy partner objektummal objektumokat?**</span><span class="sxs-lookup"><span data-stu-id="64445-138">**Q: Is it supported to soft match on-premises AD Group/Contact objects with existing Azure AD Group/Contact objects?**</span></span>  
<span data-ttu-id="64445-139">Nem, ez jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="64445-139">No, this is currently not supported.</span></span>

<span data-ttu-id="64445-140">**K:, mert az azt manuális módon állítsa be a támogatott merevlemez egyező úgy, hogy a meglévő Azure AD-csoport vagy partner objektumok attribútum ImmutableId a helyszíni AD-csoport vagy partner objektumok?**</span><span class="sxs-lookup"><span data-stu-id="64445-140">**Q: Is it supported to manually set ImmutableId attribute on existing Azure AD Group/Contact objects to hard match it to on-premises AD Group/Contact objects?**</span></span>  
<span data-ttu-id="64445-141">Nem, ez jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="64445-141">No, this is currently not supported.</span></span>



## <a name="custom-configuration"></a><span data-ttu-id="64445-142">Egyéni konfiguráció</span><span class="sxs-lookup"><span data-stu-id="64445-142">Custom configuration</span></span>
<span data-ttu-id="64445-143">**K: hol szerepelnek a PowerShell-parancsmagok az Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="64445-143">**Q: Where are the PowerShell cmdlets for Azure AD Connect documented?**</span></span>  
<span data-ttu-id="64445-144">Kivételével a parancsmagokat ezen a helyen más található az Azure AD Connect PowerShell-parancsmagok nem támogatottak a ügyfél használja.</span><span class="sxs-lookup"><span data-stu-id="64445-144">With the exception of the cmdlets documented on this site, other PowerShell cmdlets found in Azure AD Connect are not supported for customer use.</span></span>

<span data-ttu-id="64445-145">**K: használhatok "Kiszolgáló exportálási/kiszolgáló importálás" található *Synchronization Service Managert* helyezhető át a konfigurációs kiszolgáló között?**</span><span class="sxs-lookup"><span data-stu-id="64445-145">**Q: Can I use "Server export/server import" found in *Synchronization Service Manager* to move configuration between servers?**</span></span>  
<span data-ttu-id="64445-146">Nem.</span><span class="sxs-lookup"><span data-stu-id="64445-146">No.</span></span> <span data-ttu-id="64445-147">Ez a beállítás az összes konfigurációs beállítások beolvasása sikertelen lesz, és nem használható.</span><span class="sxs-lookup"><span data-stu-id="64445-147">This option will not retrieve all configuration settings and should not be used.</span></span> <span data-ttu-id="64445-148">Ehelyett használjon a varázsló az alapkonfiguráció létrehozása a második kiszolgálón, és a szerkesztővel szinkronizálási szabály létrehozása a PowerShell-parancsfájlokat egyéni szabályok áthelyezése kiszolgálók között.</span><span class="sxs-lookup"><span data-stu-id="64445-148">You should instead use the wizard to create the base configuration on the second server and use the sync rule editor to generate PowerShell scripts to move any custom rule between servers.</span></span> <span data-ttu-id="64445-149">Lásd: [áttelepítési éppen](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span><span class="sxs-lookup"><span data-stu-id="64445-149">See [Swing migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span></span>

<span data-ttu-id="64445-150">**K: kell gyorsítótárazzák a jelszavakat az Azure bejelentkezési oldal, és ez megelőzhető ugyanis tartalmaz. a jelszó bemeneti elemnek az automatikus kiegészítés = "false" attribútum?**</span><span class="sxs-lookup"><span data-stu-id="64445-150">**Q: Can passwords be cached for the Azure sign-in page and can this be prevented since it contains a password input element with the autocomplete = "false" attribute?**</span></span></br>
<span data-ttu-id="64445-151">Jelenleg nem támogatjuk a jelszó bemeneti mező, beleértve az automatikus kiegészítés címke a HTML-attribútumok módosítását.</span><span class="sxs-lookup"><span data-stu-id="64445-151">We currently do not support modifying the HTML attributes of the Password input field, including the autocomplete tag.</span></span> <span data-ttu-id="64445-152">Jelenleg dolgozunk egy szolgáltatás, amely lehetővé teszi a egyéni Javascript, amely lehetővé teszi azok az attribútumok hozzáadása a jelszó mező.</span><span class="sxs-lookup"><span data-stu-id="64445-152">We are currently working on a feature that will allow for custom Javascript which will allow you to add any attribute to the password field.</span></span> <span data-ttu-id="64445-153">2017 elérhető későbbi része legyen.</span><span class="sxs-lookup"><span data-stu-id="64445-153">This should be available later part of 2017.</span></span>

<span data-ttu-id="64445-154">**K: az Azure bejelentkezési lapon felhasználónevei, akik korábban már sikeresen bejelentkezett felhasználók jelennek meg.  Ez a viselkedés kikapcsolható?**</span><span class="sxs-lookup"><span data-stu-id="64445-154">**Q: On the Azure sign-in page, usernames for users who have previously signed in successfully are shown.  Can this behavior be turned off?**</span></span></br>
<span data-ttu-id="64445-155">Jelenleg nem támogatjuk a bejelentkezési oldal HTML attribútumainak módosítása.</span><span class="sxs-lookup"><span data-stu-id="64445-155">We currently do not support modifying the HTML attributes of the sign-in page.</span></span> <span data-ttu-id="64445-156">Jelenleg dolgozunk egy szolgáltatás, amely lehetővé teszi a egyéni Javascript, amely lehetővé teszi azok az attribútumok hozzáadása a jelszó mező.</span><span class="sxs-lookup"><span data-stu-id="64445-156">We are currently working on a feature that will allow for custom Javascript which will allow you to add any attribute to the password field.</span></span> <span data-ttu-id="64445-157">2017 elérhető későbbi része legyen.</span><span class="sxs-lookup"><span data-stu-id="64445-157">This should be available later part of 2017.</span></span>

<span data-ttu-id="64445-158">**K: akadályozza meg, hogy a munkamenetek van?**</span><span class="sxs-lookup"><span data-stu-id="64445-158">**Q: Is there a way to prevent concurrent sessions?**</span></span></br>
<span data-ttu-id="64445-159">Nem.</span><span class="sxs-lookup"><span data-stu-id="64445-159">No.</span></span>



## <a name="troubleshooting"></a><span data-ttu-id="64445-160">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="64445-160">Troubleshooting</span></span>
<span data-ttu-id="64445-161">**K: hogyan kaphat segítséget az Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="64445-161">**Q: How can I get help with Azure AD Connect?**</span></span>

[<span data-ttu-id="64445-162">A Microsoft Tudásbázisban (KB)</span><span class="sxs-lookup"><span data-stu-id="64445-162">Search the Microsoft Knowledge Base (KB)</span></span>](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* <span data-ttu-id="64445-163">Keresse meg a Microsoft Tudásbázis (KB) az Azure AD Connect-támogatással kapcsolatos gyakori javítás / csere problémákat műszaki megoldások keresése.</span><span class="sxs-lookup"><span data-stu-id="64445-163">Search the Microsoft Knowledge Base (KB) for technical solutions to common break-fix issues about Support for Azure AD Connect.</span></span>

[<span data-ttu-id="64445-164">A Microsoft Azure Active Directory-fórumok</span><span class="sxs-lookup"><span data-stu-id="64445-164">Microsoft Azure Active Directory Forums</span></span>](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* <span data-ttu-id="64445-165">Keressen, és keresse meg a technikai kérdések és a közösségi válaszok vagy saját kérdés feltevése kattintva [Itt](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).</span><span class="sxs-lookup"><span data-stu-id="64445-165">You can search and browse for technical questions and answers from the community or ask your own question by clicking [here](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).</span></span>

[<span data-ttu-id="64445-166">Az Azure AD Connect ügyfélszolgálata</span><span class="sxs-lookup"><span data-stu-id="64445-166">Azure AD Connect customer support</span></span>](https://manage.windowsazure.com/?getsupport=true)

* <span data-ttu-id="64445-167">Ez a hivatkozás segítségével segítségre van szüksége az Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="64445-167">Use this link to get support through the Azure portal.</span></span>

