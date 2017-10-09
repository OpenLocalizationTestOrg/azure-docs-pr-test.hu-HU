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
ms.openlocfilehash: 39c0b127d3dcf6f45607ad8b4647a9ad79dfc732
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a><span data-ttu-id="89830-103">Gyakori kérdések az Azure Active Directory Connect</span><span class="sxs-lookup"><span data-stu-id="89830-103">Frequently asked questions for Azure Active Directory Connect</span></span>

## <a name="general-installation"></a><span data-ttu-id="89830-104">Általános telepítési</span><span class="sxs-lookup"><span data-stu-id="89830-104">General installation</span></span>
<span data-ttu-id="89830-105">**K: telepítési működnek, ha hello Azure AD globális rendszergazda 2FA engedélyezve van?**</span><span class="sxs-lookup"><span data-stu-id="89830-105">**Q: Will installation work if hello Azure AD Global Admin has 2FA enabled?**</span></span>  
<span data-ttu-id="89830-106">A hello buildekben a 2016. februári Ez támogatott.</span><span class="sxs-lookup"><span data-stu-id="89830-106">With hello builds from February 2016, this is supported.</span></span>

<span data-ttu-id="89830-107">**K: van egy módon tooinstall felügyelet az Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="89830-107">**Q: Is there a way tooinstall Azure AD Connect unattended?**</span></span>  
<span data-ttu-id="89830-108">Már csak a támogatott tooinstall az Azure AD Connect telepítővarázsló hello használata.</span><span class="sxs-lookup"><span data-stu-id="89830-108">It is only supported tooinstall Azure AD Connect using hello installation wizard.</span></span> <span data-ttu-id="89830-109">Felügyelet nélküli és a csendes telepítés nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="89830-109">An unattended and silent installation is not supported.</span></span>

<span data-ttu-id="89830-110">**K: erdővel rendelkezem ahol tartománya nem érhető el. Hogyan kell telepíteni az Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="89830-110">**Q: I have a forest where one domain cannot be contacted. How do I install Azure AD Connect?**</span></span>  
<span data-ttu-id="89830-111">A hello buildekben a 2016. februári Ez támogatott.</span><span class="sxs-lookup"><span data-stu-id="89830-111">With hello builds from February 2016, this is supported.</span></span>

<span data-ttu-id="89830-112">**K: hello Active Directory tartományi szolgáltatások health ügynök munka a server core?**</span><span class="sxs-lookup"><span data-stu-id="89830-112">**Q: Does hello AD DS health agent work on server core?**</span></span>  
<span data-ttu-id="89830-113">Igen.</span><span class="sxs-lookup"><span data-stu-id="89830-113">Yes.</span></span> <span data-ttu-id="89830-114">Hello ügynök telepítése után hello regisztrációs folyamat a következő PowerShell-parancsmag hello segítségével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="89830-114">After installing hello agent, you can complete hello registration process using hello following PowerShell cmdlet:</span></span> 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a><span data-ttu-id="89830-115">Network (Hálózat)</span><span class="sxs-lookup"><span data-stu-id="89830-115">Network</span></span>
<span data-ttu-id="89830-116">**K: van egy tűzfal, a hálózati eszköz, vagy valami mást, amely korlátozza a hello maximális idő kapcsolatok maradhat, nyissa meg a hálózaton. Mennyi ideig kell a ügyfél ügyféloldali időkorlát küszöbértéke lehet, ha az Azure AD Connect használatával?**</span><span class="sxs-lookup"><span data-stu-id="89830-116">**Q: I have a firewall, network device, or something else that limits hello maximum time connections can stay open on my network. How long should my client side timeout threshold be when using Azure AD Connect?**</span></span>  
<span data-ttu-id="89830-117">Minden hálózati szoftvert, fizikai eszközök vagy bármely más, a kapcsolatok nyitva maradhat hello maximális idő közötti kapcsolatot kell használnia a küszöbérték legalább 5 perc (300 másodperc) korlátok hello hello Azure AD Connect-ügyfelet futtató kiszolgáló és az Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="89830-117">All networking software, physical devices, or anything else that limits hello maximum time connections can remain open should use a threshold of at least 5 minutes (300 seconds) for connectivity between hello server where hello Azure AD Connect client is installed and Azure Active Directory.</span></span> <span data-ttu-id="89830-118">Ez a korábban kiadott tooall Microsoft Identity szinkronizálási eszközöket is vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="89830-118">This also applies tooall previously released Microsoft Identity synchronization tools.</span></span>

<span data-ttu-id="89830-119">**K: vannak (egy címke tartományok) által támogatott?**</span><span class="sxs-lookup"><span data-stu-id="89830-119">**Q: Are SLDs (Single Label Domains) supported?**</span></span>  
<span data-ttu-id="89830-120">Nem, az Azure AD Connect nem támogatja a helyi erdők/tartományok által használatával.</span><span class="sxs-lookup"><span data-stu-id="89830-120">No, Azure AD Connect does not support on-premises forests/domains using SLDs.</span></span>

<span data-ttu-id="89830-121">**K: vannak "pontozott" nevű NetBios támogatott?**</span><span class="sxs-lookup"><span data-stu-id="89830-121">**Q: Are "dotted" NetBios named supported?**</span></span>  
<span data-ttu-id="89830-122">Nem, az Azure AD Connect nem támogatja a helyi erdők/tartományok ahol hello NetBIOS-név pontot tartalmaz "." hello nevében.</span><span class="sxs-lookup"><span data-stu-id="89830-122">No, Azure AD Connect does not support on-premises forests/domains where hello NetBios name contains a period "." in hello name.</span></span>

## <a name="federation"></a><span data-ttu-id="89830-123">Összevonási</span><span class="sxs-lookup"><span data-stu-id="89830-123">Federation</span></span>
<span data-ttu-id="89830-124">**K: Mi a teendő ha jelenik meg egy e-mailt, amely toorenew jóváhagyás kérése az Office 365 tanúsítvány**</span><span class="sxs-lookup"><span data-stu-id="89830-124">**Q: What do I do if I receive an email that asking me toorenew my Office 365 certificate**</span></span>  
<span data-ttu-id="89830-125">Használja a hello hello útmutatást [megújítani a tanúsítványokat](active-directory-aadconnect-o365-certs.md) című hogyan toorenew hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="89830-125">Use hello guidance that is outlined in hello [renew certificates](active-directory-aadconnect-o365-certs.md) topic on how toorenew hello certificate.</span></span>

<span data-ttu-id="89830-126">**K: van "automatikus frissítése függő" állítsa be az Office 365 függő entitáshoz. Van tootake bármely művelet, amikor a jogkivonat-aláíró tanúsítványa automatikusan visszaállítja a?**</span><span class="sxs-lookup"><span data-stu-id="89830-126">**Q: I have "Automatically update relying party" set for O365 relying party. Do I have tootake any action when my token signing certificate automatically rolls over?**</span></span>  
<span data-ttu-id="89830-127">Használja a hello cikk hello útmutatást [megújítani a tanúsítványokat](active-directory-aadconnect-o365-certs.md).</span><span class="sxs-lookup"><span data-stu-id="89830-127">Use hello guidance that is outlined in hello article [renew certificates](active-directory-aadconnect-o365-certs.md).</span></span>

## <a name="environment"></a><span data-ttu-id="89830-128">Környezet</span><span class="sxs-lookup"><span data-stu-id="89830-128">Environment</span></span>
<span data-ttu-id="89830-129">**K: az informatikai támogatott toorename hello kiszolgáló, az Azure AD Connect telepítése után?**</span><span class="sxs-lookup"><span data-stu-id="89830-129">**Q: Is it supported toorename hello server after Azure AD Connect has been installed?**</span></span>  
<span data-ttu-id="89830-130">Nem.</span><span class="sxs-lookup"><span data-stu-id="89830-130">No.</span></span> <span data-ttu-id="89830-131">Hello kiszolgáló nevének módosítása OK hello szinkronizálási motor toonot képes tooconnect toohello SQL-adatbázis és hello szolgáltatás nem lesz képes toostart.</span><span class="sxs-lookup"><span data-stu-id="89830-131">Changing hello server name will cause hello sync engine toonot be able tooconnect toohello SQL database and hello service will not be able toostart.</span></span>

## <a name="identity-data"></a><span data-ttu-id="89830-132">Azonosító adatok</span><span class="sxs-lookup"><span data-stu-id="89830-132">Identity data</span></span>
<span data-ttu-id="89830-133">**K: az Azure AD-hello UPN (userPrincipalName) attribútuma nem egyezik a helyszínen UPN - miért hello?**</span><span class="sxs-lookup"><span data-stu-id="89830-133">**Q: hello UPN (userPrincipalName) attribute in Azure AD does not match hello on-prem UPN - why?**</span></span>  
<span data-ttu-id="89830-134">Ezek a cikkek lásd:</span><span class="sxs-lookup"><span data-stu-id="89830-134">See these articles:</span></span>

* [<span data-ttu-id="89830-135">A felhasználónevek nem egyeznek az Office 365, Azure vagy Intune hello helyszíni egyszerű Felhasználónévvel vagy másodlagos bejelentkezési Azonosítóval</span><span class="sxs-lookup"><span data-stu-id="89830-135">User names in Office 365, Azure, or Intune don't match hello on-premises UPN or alternate login ID</span></span>](https://support.microsoft.com/en-us/kb/2523192)
* [<span data-ttu-id="89830-136">A felhasználói fiók toouse másik összevont tartományt egyszerű hello módosítása után a módosítások nem hello Azure Active Directory szinkronizáló eszköz által szinkronizálása</span><span class="sxs-lookup"><span data-stu-id="89830-136">Changes aren't synced by hello Azure Active Directory Sync tool after you change hello UPN of a user account toouse a different federated domain</span></span>](https://support.microsoft.com/en-us/kb/2669550)

<span data-ttu-id="89830-137">Beállíthatja úgy is az Azure AD tooallow hello szinkronizálási motor tooupdate hello userPrincipalName leírtak [az Azure AD Connect szinkronizálási szolgáltatás szolgáltatások](active-directory-aadconnectsyncservice-features.md).</span><span class="sxs-lookup"><span data-stu-id="89830-137">You can also configure Azure AD tooallow hello sync engine tooupdate hello userPrincipalName as described in [Azure AD Connect sync service features](active-directory-aadconnectsyncservice-features.md).</span></span>

<span data-ttu-id="89830-138">**K: az informatikai támogatott toosoft egyezés a helyszíni AD-csoport vagy partner objektumok meglévő Azure AD-csoport vagy partner objektummal?**</span><span class="sxs-lookup"><span data-stu-id="89830-138">**Q: Is it supported toosoft match on-premises AD Group/Contact objects with existing Azure AD Group/Contact objects?**</span></span>  
<span data-ttu-id="89830-139">Nem, ez jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="89830-139">No, this is currently not supported.</span></span>

<span data-ttu-id="89830-140">**K: az informatikai támogatott toomanually ImmutableId attribútum beállítása a meglévő Azure AD csoport vagy partner objektumok toohard egyezni tooon helyszíni AD-csoport vagy partner objektumok?**</span><span class="sxs-lookup"><span data-stu-id="89830-140">**Q: Is it supported toomanually set ImmutableId attribute on existing Azure AD Group/Contact objects toohard match it tooon-premises AD Group/Contact objects?**</span></span>  
<span data-ttu-id="89830-141">Nem, ez jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="89830-141">No, this is currently not supported.</span></span>



## <a name="custom-configuration"></a><span data-ttu-id="89830-142">Egyéni konfiguráció</span><span class="sxs-lookup"><span data-stu-id="89830-142">Custom configuration</span></span>
<span data-ttu-id="89830-143">**K: hol vannak hello PowerShell-parancsmagok az Azure AD Connect dokumentált?**</span><span class="sxs-lookup"><span data-stu-id="89830-143">**Q: Where are hello PowerShell cmdlets for Azure AD Connect documented?**</span></span>  
<span data-ttu-id="89830-144">Ezen a helyen hello parancsmagokat hello kivételével más található az Azure AD Connect PowerShell-parancsmagok nem támogatottak a ügyfél használja.</span><span class="sxs-lookup"><span data-stu-id="89830-144">With hello exception of hello cmdlets documented on this site, other PowerShell cmdlets found in Azure AD Connect are not supported for customer use.</span></span>

<span data-ttu-id="89830-145">**K: használhatok "Kiszolgáló exportálási/kiszolgáló importálás" található *Synchronization Service Managert* toomove konfigurációs kiszolgáló között?**</span><span class="sxs-lookup"><span data-stu-id="89830-145">**Q: Can I use "Server export/server import" found in *Synchronization Service Manager* toomove configuration between servers?**</span></span>  
<span data-ttu-id="89830-146">Nem.</span><span class="sxs-lookup"><span data-stu-id="89830-146">No.</span></span> <span data-ttu-id="89830-147">Ez a beállítás az összes konfigurációs beállítások beolvasása sikertelen lesz, és nem használható.</span><span class="sxs-lookup"><span data-stu-id="89830-147">This option will not retrieve all configuration settings and should not be used.</span></span> <span data-ttu-id="89830-148">Inkább helyette hello varázsló toocreate hello alapkonfiguráció hello második kiszolgálón használja és használatát hello szinkronizálási szabály szerkesztő toogenerate PowerShell parancsfájlok toomove egyéni szabályok kiszolgálók között.</span><span class="sxs-lookup"><span data-stu-id="89830-148">You should instead use hello wizard toocreate hello base configuration on hello second server and use hello sync rule editor toogenerate PowerShell scripts toomove any custom rule between servers.</span></span> <span data-ttu-id="89830-149">Lásd: [áttelepítési éppen](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span><span class="sxs-lookup"><span data-stu-id="89830-149">See [Swing migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span></span>

<span data-ttu-id="89830-150">**K: kell gyorsítótárazzák a jelszavakat az Azure-bejelentkezés hello lap, és ez megelőzhető ugyanis tartalmaz. a jelszó bemeneti elemnek hello autocomplete = "false" attribútum?**</span><span class="sxs-lookup"><span data-stu-id="89830-150">**Q: Can passwords be cached for hello Azure sign-in page and can this be prevented since it contains a password input element with hello autocomplete = "false" attribute?**</span></span></br>
<span data-ttu-id="89830-151">Jelenleg nem támogatjuk hello jelszó beviteli mezőt, beleértve a hello autocomplete címke módosítása hello HTML attribútumait.</span><span class="sxs-lookup"><span data-stu-id="89830-151">We currently do not support modifying hello HTML attributes of hello Password input field, including hello autocomplete tag.</span></span> <span data-ttu-id="89830-152">Jelenleg dolgozunk egy szolgáltatás, amely lehetővé teszi a egyéni Javascript, amely lehetővé teszi a tooadd bármely attribútum toohello jelszó mező.</span><span class="sxs-lookup"><span data-stu-id="89830-152">We are currently working on a feature that will allow for custom Javascript which will allow you tooadd any attribute toohello password field.</span></span> <span data-ttu-id="89830-153">2017 elérhető későbbi része legyen.</span><span class="sxs-lookup"><span data-stu-id="89830-153">This should be available later part of 2017.</span></span>

<span data-ttu-id="89830-154">**K: az Azure bejelentkezési oldal hello a felhasználók számára, akik korábban már sikeresen bejelentkezett felhasználónevek jelennek meg.  Ez a viselkedés kikapcsolható?**</span><span class="sxs-lookup"><span data-stu-id="89830-154">**Q: On hello Azure sign-in page, usernames for users who have previously signed in successfully are shown.  Can this behavior be turned off?**</span></span></br>
<span data-ttu-id="89830-155">Jelenleg nem támogatott módosítása hello hello bejelentkezési lap HTML-attribútumok.</span><span class="sxs-lookup"><span data-stu-id="89830-155">We currently do not support modifying hello HTML attributes of hello sign-in page.</span></span> <span data-ttu-id="89830-156">Jelenleg dolgozunk egy szolgáltatás, amely lehetővé teszi a egyéni Javascript, amely lehetővé teszi a tooadd bármely attribútum toohello jelszó mező.</span><span class="sxs-lookup"><span data-stu-id="89830-156">We are currently working on a feature that will allow for custom Javascript which will allow you tooadd any attribute toohello password field.</span></span> <span data-ttu-id="89830-157">2017 elérhető későbbi része legyen.</span><span class="sxs-lookup"><span data-stu-id="89830-157">This should be available later part of 2017.</span></span>

<span data-ttu-id="89830-158">**K: van egy módon tooprevent egyidejű munkamenetek?**</span><span class="sxs-lookup"><span data-stu-id="89830-158">**Q: Is there a way tooprevent concurrent sessions?**</span></span></br>
<span data-ttu-id="89830-159">Nem.</span><span class="sxs-lookup"><span data-stu-id="89830-159">No.</span></span>



## <a name="troubleshooting"></a><span data-ttu-id="89830-160">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="89830-160">Troubleshooting</span></span>
<span data-ttu-id="89830-161">**K: hogyan kaphat segítséget az Azure AD Connect?**</span><span class="sxs-lookup"><span data-stu-id="89830-161">**Q: How can I get help with Azure AD Connect?**</span></span>

[<span data-ttu-id="89830-162">Keresési hello Microsoft Tudásbázis (KB)</span><span class="sxs-lookup"><span data-stu-id="89830-162">Search hello Microsoft Knowledge Base (KB)</span></span>](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* <span data-ttu-id="89830-163">Keresse meg a Microsoft Tudásbázis (KB) hello műszaki megoldások toocommon javítás / csere problémák az Azure AD Connect-támogatással kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="89830-163">Search hello Microsoft Knowledge Base (KB) for technical solutions toocommon break-fix issues about Support for Azure AD Connect.</span></span>

[<span data-ttu-id="89830-164">A Microsoft Azure Active Directory-fórumok</span><span class="sxs-lookup"><span data-stu-id="89830-164">Microsoft Azure Active Directory Forums</span></span>](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* <span data-ttu-id="89830-165">Keressen, és keresse meg a technikai kérdések és hello Közösségtől felveszi vagy saját kérdés feltevése kattintva [Itt](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).</span><span class="sxs-lookup"><span data-stu-id="89830-165">You can search and browse for technical questions and answers from hello community or ask your own question by clicking [here](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).</span></span>

[<span data-ttu-id="89830-166">Az Azure AD Connect ügyfélszolgálata</span><span class="sxs-lookup"><span data-stu-id="89830-166">Azure AD Connect customer support</span></span>](https://manage.windowsazure.com/?getsupport=true)

* <span data-ttu-id="89830-167">Használja a kapcsolat tooget támogatás hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="89830-167">Use this link tooget support through hello Azure portal.</span></span>

