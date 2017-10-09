---
title: "Gyakran ismételt kérdések: Azure AD SSPR |} Microsoft Docs"
description: "Gyakori kérdések az Azure AD az önkiszolgáló jelszó alaphelyzetbe állítása"
services: active-directory
keywords: "Az Active directory-jelszókezelés, jelszókezelés, az Azure AD self service jelszó alaphelyzetbe állítása"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: d04a9efeb3b35421aa605cadb2aa25f656a4d515
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="password-management-frequently-asked-questions"></a><span data-ttu-id="1dff0-104">A jelszókezelés gyakran ismételt kérdések</span><span class="sxs-lookup"><span data-stu-id="1dff0-104">Password management frequently asked questions</span></span>

<span data-ttu-id="1dff0-105">a következő hello néhány gyakori kérdés, minden kapcsolódó toopassword alaphelyzetbe.</span><span class="sxs-lookup"><span data-stu-id="1dff0-105">hello following are some frequently asked questions for all things related toopassword reset.</span></span>

<span data-ttu-id="1dff0-106">Ha az Azure AD általános kérdése van, és az önkiszolgáló jelszó alaphelyzetbe állítása, nem megválaszolandó itt, megkérheti hello közösségi Ha segítségre van szüksége a hello [Azure Ad-fórumok](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD).</span><span class="sxs-lookup"><span data-stu-id="1dff0-106">If you have a general question about Azure AD and self-service password reset, that is not answered here, you can ask hello community for assistance on hello [Azure Ad forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD).</span></span> <span data-ttu-id="1dff0-107">Hello Közösség tagjai közé tartozik a mérnökök, termék kezelők, MVP és ösztöndíjas informatikai szakemberek számára.</span><span class="sxs-lookup"><span data-stu-id="1dff0-107">Members of hello community include Engineers, Product Managers, MVPs, and fellow IT Professionals.</span></span>

<span data-ttu-id="1dff0-108">Ez a GYIK a következő szakaszok hello oszlik:</span><span class="sxs-lookup"><span data-stu-id="1dff0-108">This FAQ is split into hello following sections:</span></span>

* [<span data-ttu-id="1dff0-109">**A Jelszóátállítás regisztrációját kapcsolatos kérdések**</span><span class="sxs-lookup"><span data-stu-id="1dff0-109">**Questions about Password Reset Registration**</span></span>](#password-reset-registration)
* [<span data-ttu-id="1dff0-110">**Jelszó alaphelyzetbe állítása kérdések**</span><span class="sxs-lookup"><span data-stu-id="1dff0-110">**Questions about Password Reset**</span></span>](#password-reset)
* [<span data-ttu-id="1dff0-111">**A jelszó módosítása kapcsolatos kérdések**</span><span class="sxs-lookup"><span data-stu-id="1dff0-111">**Questions about Password Change**</span></span>](#password-change)
* [<span data-ttu-id="1dff0-112">**Tudnivalók a jelszókezelésről kérdések jelentések**</span><span class="sxs-lookup"><span data-stu-id="1dff0-112">**Questions about password management Reports**</span></span>](#password-management-reports)
* [<span data-ttu-id="1dff0-113">**A jelszóvisszaírás kapcsolatos kérdések**</span><span class="sxs-lookup"><span data-stu-id="1dff0-113">**Questions about password writeback**</span></span>](#password-writeback)

## <a name="password-reset-registration"></a><span data-ttu-id="1dff0-114">Jelszó-átállítási regisztrációk</span><span class="sxs-lookup"><span data-stu-id="1dff0-114">Password reset registration</span></span>
* <span data-ttu-id="1dff0-115">**K: a felhasználók regisztrálhatják saját jelszó alaphelyzetbe állítása adatokat?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-115">**Q:  Can my users register their own password reset data?**</span></span>

  > <span data-ttu-id="1dff0-116">**V:** Igen, mindaddig, amíg engedélyezve van a jelszó alaphelyzetbe állítása, és azok licencét, akkor lépjen toohello jelszó-átállítási regisztráció portálon http://aka.ms/ssprsetup tooregister, a hitelesítési adatokat.</span><span class="sxs-lookup"><span data-stu-id="1dff0-116">**A:** Yes, as long as password reset is enabled and they are licensed, they can go toohello Password Reset Registration portal at http://aka.ms/ssprsetup tooregister their authentication information.</span></span> <span data-ttu-id="1dff0-117">A felhasználók is regisztrálhatják által toohello hozzáférési panelre lesz a http://myapps.microsoft.com, hello-profil lapon, és kattintson a hello regisztrálása a jelszó alaphelyzetbe állítása lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="1dff0-117">Users can also register by going toohello access panel at http://myapps.microsoft.com, clicking hello profile tab, and clicking hello Register for Password Reset option.</span></span>
  >
  >
* <span data-ttu-id="1dff0-118">**K: jelszó alaphelyzetbe állítása adatok is definiálása a felhasználók nevében?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-118">**Q:  Can I define password reset data on behalf of my users?**</span></span>

  > <span data-ttu-id="1dff0-119">**V:** Igen, akkor az Azure AD Connect PowerShell, hello [Azure-portálon](https://portal.azure.com), vagy hello Office felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="1dff0-119">**A:** Yes, you can do so with Azure AD Connect, PowerShell, hello [Azure portal](https://portal.azure.com), or hello Office Admin portal.</span></span> <span data-ttu-id="1dff0-120">További információkért lásd: hello cikk [az Azure AD önkiszolgáló jelszó-változtatási által használt adatok](active-directory-passwords-data.md).</span><span class="sxs-lookup"><span data-stu-id="1dff0-120">For more information, see hello article [Data used by Azure AD Self-Service Password Reset](active-directory-passwords-data.md).</span></span>
  >
  >
* <span data-ttu-id="1dff0-121">**K: biztonsági kérdéseket tesz fel a helyszíni adatok szinkronizálása?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-121">**Q:  Can I synchronize data for security questions from on premises?**</span></span>

  > <span data-ttu-id="1dff0-122">**V:** erre nincs lehetőség ma.</span><span class="sxs-lookup"><span data-stu-id="1dff0-122">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="1dff0-123">**K: a felhasználók regisztrálhatják adatokat úgy, hogy más felhasználók nem láthatják ezeket az adatokat?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-123">**Q:  Can my users register data in such a way that other users cannot see this data?**</span></span>

  > <span data-ttu-id="1dff0-124">**V:** Igen, ha a felhasználók regisztrálhatnak hello jelszó-változtatási regisztrációs portálra a Mentés személyes hitelesítési mezőkbe, amelyek csak a globális rendszergazdák és hello felhasználó által látható adatokat.</span><span class="sxs-lookup"><span data-stu-id="1dff0-124">**A:** Yes, when users register data using hello Password Reset Registration Portal it is saved into private authentication fields that are only visible by Global Administrators and hello user.</span></span>
    >
    > [!NOTE]
    > <span data-ttu-id="1dff0-125">Ha egy **Azure rendszergazdai fiók** regisztrálja a hitelesítéshez használni kívánt telefonszámot is gyűjteménytípust hello mobiltelefon mezőbe, és látható.</span><span class="sxs-lookup"><span data-stu-id="1dff0-125">If an **Azure Administrator account** registers their authentication phone number it is also populated into hello mobile phone field and is visible.</span></span>
    >
  >
  >
* <span data-ttu-id="1dff0-126">**K: a felhasználók rendelkeznek toobe regisztrálva a jelszó alaphelyzetbe állítása használatához?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-126">**Q:  Do my users have toobe registered before they can use password reset?**</span></span>

  > <span data-ttu-id="1dff0-127">**V:** nem, ha elegendő hitelesítési adatokat definiálja a nevében, felhasználó nem rendelkezik tooregister.</span><span class="sxs-lookup"><span data-stu-id="1dff0-127">**A:** No, if you define enough authentication information on their behalf, users do not have tooregister.</span></span> <span data-ttu-id="1dff0-128">Jelszó-átállítási működik, amíg a megfelelően formázott adatok hello megfelelő mezőkben hello könyvtárban tárolja.</span><span class="sxs-lookup"><span data-stu-id="1dff0-128">Password reset works as long as you have properly formatted data stored in hello appropriate fields in hello directory.</span></span>
  >
  >
* <span data-ttu-id="1dff0-129">**K: I szinkronizálása vagy hello hitelesítéshez megadott telefonját, hitelesítési E-mail vagy alternatív hitelesítéshez megadott telefonját mezők beállítása a felhasználók nevében?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-129">**Q:  Can I synchronize or set hello Authentication Phone, Authentication Email or Alternate Authentication Phone fields on behalf of my users?**</span></span>

  > <span data-ttu-id="1dff0-130">**V:** erre nincs lehetőség ma.</span><span class="sxs-lookup"><span data-stu-id="1dff0-130">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="1dff0-131">**K: hogyan nem hello regisztrációs portálon, hogy mely beállítások tooshow a felhasználók?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-131">**Q:  How does hello registration portal know which options tooshow my users?**</span></span>

  > <span data-ttu-id="1dff0-132">**V:** hello jelszó-változtatási regisztrációs portálra csak azt mutatja be, hogy a felhasználók számára engedélyezett beállítások hello.</span><span class="sxs-lookup"><span data-stu-id="1dff0-132">**A:** hello password reset registration portal only shows hello options that you have enabled for your users.</span></span> <span data-ttu-id="1dff0-133">Ezek a beállítások hello a könyvtár konfigurálása lap felhasználói jelszó-visszaállítási házirend szakasza alatt találhatók. Például ez azt jelenti, hogy ha nem engedélyezi a biztonsági kérdéseket, majd felhasználók nem tud tooregister az adott beállítási mód.</span><span class="sxs-lookup"><span data-stu-id="1dff0-133">These options are found under hello User Password Reset Policy section of your directory’s Configure tab. For example, this means that if you do not enable security questions, then users are not able tooregister for that option.</span></span>
  >
  >
* <span data-ttu-id="1dff0-134">**Amikor egy felhasználó tekinthető k: regisztrálni?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-134">**Q:  When is a user considered registered?**</span></span>

  > <span data-ttu-id="1dff0-135">**V:** regisztrált sspr, amikor regisztrálják azokat A felhasználó akkor tekinthető legalább hello **módszerek szükséges tooreset száma** hello beállított [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1dff0-135">**A:** A user is considered registered for SSPR when they have registered at least hello **Number of methods required tooreset** that you have set in hello [Azure portal](https://portal.azure.com).</span></span>
  >
  >
## <a name="password-reset"></a><span data-ttu-id="1dff0-136">Új jelszó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1dff0-136">Password reset</span></span>
* <span data-ttu-id="1dff0-137">**K: mennyi ideig I várjon tooreceive egy e-mailek, SMS vagy a jelszó alaphelyzetbe állítása a telefonhívás?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-137">**Q:  How long should I wait tooreceive an email, SMS, or phone call from password reset?**</span></span>

  > <span data-ttu-id="1dff0-138">**V:** e-mailben, SMS-t, és telefonhívásokat egy perc alatt 5-20 másodperc alatt hello normál esetben kell érkeznek.</span><span class="sxs-lookup"><span data-stu-id="1dff0-138">**A:** Email, SMS messages, and phone calls should arrive in under one minute, with hello normal case being 5-20 seconds.</span></span>
    ><span data-ttu-id="1dff0-139">Ha nem ez időkereten belül hello értesítés jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="1dff0-139">If you do not receive hello notification in this time frame:</span></span>
        > * <span data-ttu-id="1dff0-140">Ellenőrizze a Levélszemét mappát.</span><span class="sxs-lookup"><span data-stu-id="1dff0-140">Check your junk folder.</span></span>
        > * <span data-ttu-id="1dff0-141">Ellenőrzése hello vagy e-mail, amelyhez csatlakozik egy várt hello.</span><span class="sxs-lookup"><span data-stu-id="1dff0-141">Check hello number or email being contacted is hello one you expect.</span></span>
        > * <span data-ttu-id="1dff0-142">Ellenőrizze, hogy az hello directory hello hitelesítési adatai megfelelően van formázva.</span><span class="sxs-lookup"><span data-stu-id="1dff0-142">Check hello authentication data in hello directory is correctly formatted.</span></span>
                >     * <span data-ttu-id="1dff0-143">Példa: "+ 1 4255551234" vagy "user@contoso.com"</span><span class="sxs-lookup"><span data-stu-id="1dff0-143">Example: "+1 4255551234" or "user@contoso.com"</span></span>
  >
  >
* <span data-ttu-id="1dff0-144">**K: jelszó alaphelyzetbe állítása milyen nyelveket támogatja?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-144">**Q:  What languages are supported by password reset?**</span></span>

  > <span data-ttu-id="1dff0-145">**V:** hello a jelszó alaphelyzetbe állítása felhasználói felület, SMS-t, és hangra vonatkozó hívások honosítva hello ugyanazt az Office 365-ben támogatott nyelvek.</span><span class="sxs-lookup"><span data-stu-id="1dff0-145">**A:** hello password reset UI, SMS messages, and voice calls are localized in hello same languages that are supported in Office 365.</span></span>
  >
  >
* <span data-ttu-id="1dff0-146">**K: milyen hello jelszó alaphelyzetbe állítása élmény részei beolvasása vállalati arculattal, szervezeti a címtárban branding meg tartozó konfigurálása lapon?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-146">**Q:  What parts of hello password reset experience get branded when I set organizational branding in my directory’s configure tab?**</span></span>

  > <span data-ttu-id="1dff0-147">**V:** hello jelszó-változtatási portál jeleníti meg a szervezeti embléma, és lehetővé teszi a tooconfigure hello kérje a rendszergazda kapcsolat toopoint tooa egyéni e-mail vagy URL-címe.</span><span class="sxs-lookup"><span data-stu-id="1dff0-147">**A:** hello password reset portal shows your organizational logo and allows you tooconfigure hello Contact your administrator link toopoint tooa custom email or URL.</span></span> <span data-ttu-id="1dff0-148">Minden e-mailben küldi el jelszóvisszaállítás üdvözlő e-mail törzsét hello a vállalati embléma, a színeket, a nevét tartalmazza, és testre neve.</span><span class="sxs-lookup"><span data-stu-id="1dff0-148">Any email that gets sent by password reset includes your organization’s logo, colors, name in hello body of hello email, and customized from name.</span></span>
  >
  >
* <span data-ttu-id="1dff0-149">**K: hogyan lehet tájékoztassa a felhasználókat a where toogo tooreset jelszavukat?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-149">**Q:  How can I educate my users about where toogo tooreset their passwords?**</span></span>

  > <span data-ttu-id="1dff0-150">**V:** is elküldheti a felhasználók toohttps://passwordreset.microsoftonline.com közvetlenül, vagy utasíthatja őket tooclick hello **nem tudja elérni a fiókot hivatkozás** bármely munkahelyi vagy iskolai bejelentkezési lapján található.</span><span class="sxs-lookup"><span data-stu-id="1dff0-150">**A:** You can send your users toohttps://passwordreset.microsoftonline.com directly, or you can instruct them tooclick hello **Can’t access your account link** found on any Work or School sign-in page.</span></span> <span data-ttu-id="1dff0-151">Egy hely könnyen hozzáférhető tooyour felhasználók is közzéteheti ezeket a hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="1dff0-151">You can also publish these links in a place easily accessible tooyour users.</span></span>
  >
  >
* <span data-ttu-id="1dff0-152">**K: használhatok ezen a lapon egy mobileszközről?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-152">**Q:  Can I use this page from a mobile device?**</span></span>

  > <span data-ttu-id="1dff0-153">**V:** Igen, az ezen a lapon a mobileszközök működik.</span><span class="sxs-lookup"><span data-stu-id="1dff0-153">**A:** Yes, this page works on mobile devices.</span></span>
  >
  >
* <span data-ttu-id="1dff0-154">**K: támogatják a feloldásának helyi active directory-fiókokat, amikor a felhasználók visszaállíthassák a jelszavukat?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-154">**Q:  Do you support unlocking local active directory accounts when users reset their passwords?**</span></span>

  > <span data-ttu-id="1dff0-155">**V:** Igen, ha a felhasználó visszaállítja a jelszavát, és a jelszóvisszaíró az Azure AD Connect használatával lett telepítve, adott felhasználói fiók esetén automatikusan feloldani a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="1dff0-155">**A:** Yes, when a user resets their password and password writeback has been deployed using Azure AD Connect, that user’s account is automatically unlocked when they reset their password.</span></span>
  >
  >
* <span data-ttu-id="1dff0-156">**K: hogyan integrálhatók a jelszó alaphelyzetbe állítása közvetlenül a saját felhasználói asztali bejelentkezés során tapasztal élmény?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-156">**Q:  How can I integrate password reset directly into my user’s desktop sign-in experience?**</span></span>

  > <span data-ttu-id="1dff0-157">**V:** Ha az Azure AD Premium ügyfél egy, a Microsoft Identity Manager további költségek nélkül telepítheti és hello helyszíni jelszó alaphelyzetbe állítása megoldás toomeet telepíteni ezt a követelményt.</span><span class="sxs-lookup"><span data-stu-id="1dff0-157">**A:** If you are an Azure AD Premium customer, you can install Microsoft Identity Manager at no additional cost and deploy hello on-premises password reset solution toomeet this requirement.</span></span>
  >
  >
* <span data-ttu-id="1dff0-158">**K: beállítása a különböző területi beállításokhoz különböző biztonsági kérdést?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-158">**Q:  Can I set different security questions for different locales?**</span></span>

  > <span data-ttu-id="1dff0-159">**V:** erre nincs lehetőség ma.</span><span class="sxs-lookup"><span data-stu-id="1dff0-159">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="1dff0-160">**K: hogyan több kérdést azt konfigurálható hello biztonsági kérdések hitelesítési lehetőséget?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-160">**Q:  How many questions can we configure for hello Security Questions authentication option?**</span></span>

  > <span data-ttu-id="1dff0-161">**V:** meg too20 egyéni biztonsági kérdéseket a hello konfigurálható [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1dff0-161">**A:** You can configure up too20 custom security questions in hello [Azure portal](https://portal.azure.com).</span></span>
  >
  >
* <span data-ttu-id="1dff0-162">**K: mennyi ideig lehet, hogy a biztonsági kérdések kell?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-162">**Q:  How long may security questions be?**</span></span>

  > <span data-ttu-id="1dff0-163">**V:** biztonsági kérdések 3 és 200 karakter közötti lehet.</span><span class="sxs-lookup"><span data-stu-id="1dff0-163">**A:** Security questions may be between 3 and 200 characters long.</span></span>
  >
  >
* <span data-ttu-id="1dff0-164">**K: mennyi ideig toosecurity kérdésekre adott válaszok lehet?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-164">**Q:  How long may answers toosecurity questions be?**</span></span>

  > <span data-ttu-id="1dff0-165">**V:** válaszok 3 too40 karakter hosszú lehet.</span><span class="sxs-lookup"><span data-stu-id="1dff0-165">**A:** Answers may be 3 too40 characters long.</span></span>
  >
  >
* <span data-ttu-id="1dff0-166">**K: ismétlődő válaszokat toosecurity kérdések utasítja el?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-166">**Q:  Are duplicate answers toosecurity questions rejected?**</span></span>

  > <span data-ttu-id="1dff0-167">**V:** Igen, azt utasítsa el az ismétlődő válaszokat toosecurity kérdéseket.</span><span class="sxs-lookup"><span data-stu-id="1dff0-167">**A:** Yes, we reject duplicate answers toosecurity questions.</span></span>
  >
  >
* <span data-ttu-id="1dff0-168">**K: Előfordulhat, hogy a felhasználó regisztrálása hello ugyanazt biztonsági kérdés egynél többször?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-168">**Q:  May a user register hello same security question more than once?**</span></span>

  > <span data-ttu-id="1dff0-169">**V:** nem, miután a felhasználó regisztrál egy adott kérdést, előfordulhat, hogy nem regisztrálják az adott kérdést, még egyszer.</span><span class="sxs-lookup"><span data-stu-id="1dff0-169">**A:** No, once a user registers a particular question, they may not register for that question a second time.</span></span>
  >
  >
* <span data-ttu-id="1dff0-170">**K: az azt a biztonsági kérdések regisztrációs és alaphelyzetbe állítása a minimális korlát lehetséges tooset?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-170">**Q:  Is it possible tooset a minimum limit of security questions for registration and reset?**</span></span>

  > <span data-ttu-id="1dff0-171">**V:** Igen, egy korlát a regisztrációs és egy másik alaphelyzetbe állítása a állítható be.</span><span class="sxs-lookup"><span data-stu-id="1dff0-171">**A:** Yes, one limit can be set for registration and another for reset.</span></span> <span data-ttu-id="1dff0-172">lehet, hogy 3-5 biztonsági kérdések regisztrációjához szükséges, és lehet, hogy 3-5 alaphelyzetbe állítása szükséges.</span><span class="sxs-lookup"><span data-stu-id="1dff0-172">3-5 security questions may be required for registration and 3-5 may be required for reset.</span></span>
  >
  >
* <span data-ttu-id="1dff0-173">**Kérdés: Ha egy felhasználó több hello maximális kérdések szükséges tooreset regisztrálva van, hogy miként kell a biztonsági kérdések kijelölni alaphelyzetbe állítása során?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-173">**Q:  If a user has registered more than hello maximum number of questions required tooreset, how are security questions selected during reset?**</span></span>

  > <span data-ttu-id="1dff0-174">**V:** N biztonsági kérdések véletlenszerűen kiválasztott kívül hello teljes száma a felhasználói kérdések regisztrálva van, ahol N az hello **kérdések szükséges tooreset száma**.</span><span class="sxs-lookup"><span data-stu-id="1dff0-174">**A:** N security questions are selected at random out of hello total number of questions a user has registered for, where N is hello **Number of questions required tooreset**.</span></span> <span data-ttu-id="1dff0-175">Például ha egy felhasználó rendelkezik-e regisztrálva 5 biztonsági kérdéseit, de csak 3 szükséges tooreset, hello 5 3 véletlenszerűen és jelenik meg a következő alaphelyzetbe állítása.</span><span class="sxs-lookup"><span data-stu-id="1dff0-175">For example, if a user has 5 security questions registered, but only 3 are required tooreset, 3 of hello 5 are selected randomly and presented at reset.</span></span> <span data-ttu-id="1dff0-176">Hello felhasználó élvezheti hello válaszok toohello kérdések helytelen, ha a hello tanúsítványkiválasztási folyamat tooprevent kérdés beütés jelentkezik.</span><span class="sxs-lookup"><span data-stu-id="1dff0-176">If hello user gets hello answers toohello questions wrong, hello selection process reoccurs tooprevent question hammering.</span></span>
  >
  >
* <span data-ttu-id="1dff0-177">**K: el azt, hogy felhasználók jelszó-változtatási rövid időn belül időn belül többször próbálnak?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-177">**Q:  Do you prevent users from attempting password reset many times in a short time period?**</span></span>

  > <span data-ttu-id="1dff0-178">**V:** Igen, vannak beépítve a jelszó alaphelyzetbe állítása tooprotect visszaélés elleni funkciókat.</span><span class="sxs-lookup"><span data-stu-id="1dff0-178">**A:** Yes, there are security features built into password reset tooprotect from misuse.</span></span> <span data-ttu-id="1dff0-179">Előfordulhat, hogy a felhasználók csak megpróbálják 5 jelszó alaphelyzetbe állítása kísérletek 24 óra alatt zárolása előtt egy órán belül.</span><span class="sxs-lookup"><span data-stu-id="1dff0-179">Users may only try 5 password reset attempts within an hour before being locked out for 24 hours.</span></span> <span data-ttu-id="1dff0-180">A felhasználók előfordulhat, hogy csak megpróbálják toovalidate telefonszám 5-ször 24 óra alatt zárolása előtt egy órán belül.</span><span class="sxs-lookup"><span data-stu-id="1dff0-180">Users may only try toovalidate a phone number 5 times within an hour before being locked out for 24 hours.</span></span> <span data-ttu-id="1dff0-181">A felhasználók csak lehet, hogy megpróbálják egy egyetlen hitelesítési módszer 24 óra alatt zárolása előtt egy órán belül 5 alkalommal.</span><span class="sxs-lookup"><span data-stu-id="1dff0-181">Users may only try a single authentication method 5 times within an hour before being locked out for 24 hours.</span></span>
  >
  >
* <span data-ttu-id="1dff0-182">**K:, hogy mennyi ideig érvényesek hello e-mailek és SMS egyszer használatos jelszót?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-182">**Q:  For how long are hello email and SMS one-time passcode valid?**</span></span>

  > <span data-ttu-id="1dff0-183">**V:** hello munkamenetek élettartamát, jelszó-visszaállításhoz érték 105 perc.</span><span class="sxs-lookup"><span data-stu-id="1dff0-183">**A:** hello session lifetime for password reset is 105 minutes.</span></span> <span data-ttu-id="1dff0-184">Hello hello kezdetén jelszó-átállítási művelet, hello felhasználó rendelkezik 105 perc tooreset a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="1dff0-184">From hello beginning of hello password reset operation, hello user has 105 minutes tooreset their password.</span></span> <span data-ttu-id="1dff0-185">hello e-mailek és SMS egyszer használatos jelszót érvénytelenek ebben az időszakban lejárata után is.</span><span class="sxs-lookup"><span data-stu-id="1dff0-185">hello email and SMS one-time passcode are invalid after this time period expires.</span></span>
  >
  >

## <a name="password-change"></a><span data-ttu-id="1dff0-186">A jelszó módosítása</span><span class="sxs-lookup"><span data-stu-id="1dff0-186">Password change</span></span>
* <span data-ttu-id="1dff0-187">**K: kell a felhasználók hová toochange jelszavukat?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-187">**Q:  Where should my users go toochange their passwords?**</span></span>

  > <span data-ttu-id="1dff0-188">**V:** felhasználók előfordulhat, hogy módosítsák jelszavukat bárhol akkor jelenik meg a profil kép vagy ikon (a hello jobb felső sarkában, például a [Office 365](https://portal.office.com) vagy [hozzáférési Panel](https://myapps.microsoft.com) észlel.</span><span class="sxs-lookup"><span data-stu-id="1dff0-188">**A:** Users may change their passwords anywhere they see their profile picture or icon (like in hello upper right corner of their [Office 365](https://portal.office.com) or [Access Panel](https://myapps.microsoft.com) experiences.</span></span> <span data-ttu-id="1dff0-189">Felhasználók a jelszavuk változhat hello [hozzáférési Panel profilszerkesztési lap](https://account.activedirectory.windowsazure.com/r#/profile).</span><span class="sxs-lookup"><span data-stu-id="1dff0-189">Users may change their passwords from hello [Access Panel profile page](https://account.activedirectory.windowsazure.com/r#/profile).</span></span> <span data-ttu-id="1dff0-190">Felhasználók is lehet, hogy az alkalmazás megkéri toochange automatikusan hello Azure AD bejelentkezési képernyő, a jelszavuk jelszavuk.</span><span class="sxs-lookup"><span data-stu-id="1dff0-190">Users may also be asked toochange their passwords automatically at hello Azure AD sign-in screen if their passwords have expired.</span></span> <span data-ttu-id="1dff0-191">Végezetül, a felhasználók megnyitják toohello előfordulhat, hogy [Azure AD-jelszó módosítása Portal](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) közvetlenül a jelszavuk alapértékekkel toochange.</span><span class="sxs-lookup"><span data-stu-id="1dff0-191">Finally, users may navigate toohello [Azure AD Password Change Portal](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) directly if they wish toochange their passwords.</span></span>
  >
  >
* <span data-ttu-id="1dff0-192">**K: a felhasználók értesítést kaphat az Office portál hello helyszíni jelszavukat lejárati?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-192">**Q:  Can my users be notified in hello Office Portal when their on-premises password expires?**</span></span>

  > <span data-ttu-id="1dff0-193">**V:** Ez azért lehetséges ma használata az AD FS itt hello utasításokat követve: [küldése jelszó házirend jogcímek az AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396).</span><span class="sxs-lookup"><span data-stu-id="1dff0-193">**A:** This is possible today if you are using ADFS by following hello instructions here: [Sending Password Policy Claims with ADFS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396).</span></span> <span data-ttu-id="1dff0-194">Ha a Jelszókivonat-szinkronizálást használ, ez nem lehetséges ma.</span><span class="sxs-lookup"><span data-stu-id="1dff0-194">If you are using password hash synchronization, this is not possible today.</span></span> <span data-ttu-id="1dff0-195">Ennek az az oka, hogy szinkronizálja a helyszíni jelszóházirendek, ezért nem lehetséges az USA toopost lejárati értesítések toocloud észlel.</span><span class="sxs-lookup"><span data-stu-id="1dff0-195">This is because we do not sync password policies from on-premises, so it is not possible for us toopost expiry notifications toocloud experiences.</span></span> <span data-ttu-id="1dff0-196">Mindkét esetben is lehetőség túl[értesítse a felhasználókat, amelyeknek a jelszavánál tooexpire kerül a PowerShell használatával](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).</span><span class="sxs-lookup"><span data-stu-id="1dff0-196">In either case, it is also possible too[notify users whose passwords are about tooexpire by using PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).</span></span>
  >
  >

## <a name="password-management-reports"></a><span data-ttu-id="1dff0-197">Jelszó-kezelési jelentések</span><span class="sxs-lookup"><span data-stu-id="1dff0-197">Password management reports</span></span>
* <span data-ttu-id="1dff0-198">**K: mennyi időt vesz igénybe az adatok tooshow mentése hello jelszó felügyeleti jelentésekre?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-198">**Q:  How long does it take for data tooshow up on hello password management reports?**</span></span>

  > <span data-ttu-id="1dff0-199">**V:** adatokat meg kell jelennie hello jelszó jelentések 5-10 percen belül.</span><span class="sxs-lookup"><span data-stu-id="1dff0-199">**A:** Data should appear on hello password management reports within 5-10 minutes.</span></span> <span data-ttu-id="1dff0-200">Az egyes példányok tooan óra tooappear igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="1dff0-200">It some instances it may take up tooan hour tooappear.</span></span>
  >
  >
* <span data-ttu-id="1dff0-201">**K: hogyan végezhet hello jelszó jelentések?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-201">**Q:  How can I filter hello password management reports?**</span></span>

  > <span data-ttu-id="1dff0-202">**V:** hello jelszó jelentések hello kis nagyítóüveg toohello jobb hello oszlop címkék hello jelentés hello tetején kattintva szűrheti is.</span><span class="sxs-lookup"><span data-stu-id="1dff0-202">**A:** You can filter hello password management reports by clicking hello small magnifying glass toohello extreme right of hello column labels, near hello top of hello report.</span></span> <span data-ttu-id="1dff0-203">Ha azt szeretné, hogy toodo gazdagabb szűrés, hello jelentés tooexcel letöltheti és kimutatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="1dff0-203">If you want toodo richer filtering, you can download hello report tooexcel and create a pivot table.</span></span>
  >
  >
* <span data-ttu-id="1dff0-204">**K: Mi az az események maximális száma hello hello jelszó jelentések tárolódnak?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-204">**Q: What is hello maximum number of events are stored in hello password management reports?**</span></span>

  > <span data-ttu-id="1dff0-205">**V:** be too75, 000 jelszó alaphelyzetbe állítása vagy a jelszó alaphelyzetbe állítása regisztrációs események hello jelszó felügyeleti jelentések, készítsen biztonsági másolatot too30 nap átfedés vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="1dff0-205">**A:** Up too75,000 password reset or password reset registration events are stored in hello password management reports, spanning back up too30 days.</span></span>  <span data-ttu-id="1dff0-206">Dolgozunk ennek tooexpand ez tooinclude további események száma.</span><span class="sxs-lookup"><span data-stu-id="1dff0-206">We are working tooexpand this number tooinclude more events.</span></span>
  >
  >
* <span data-ttu-id="1dff0-207">**K: milyen távolságban vissza hello jelszó jelentések Ugrás?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-207">**Q:  How far back do hello password management reports go?**</span></span>

  > <span data-ttu-id="1dff0-208">**V:** hello jelszókezelés jelentések megjelenítése műveletek hello belül előforduló utolsó 30 nap.</span><span class="sxs-lookup"><span data-stu-id="1dff0-208">**A:** hello password management reports show operations occurring within hello last 30 days.</span></span> <span data-ttu-id="1dff0-209">A lépést Ha ezek az adatok kell tooarchive meg is letöltheti hello jelentések rendszeres időközönként és mentheti azokat külön.</span><span class="sxs-lookup"><span data-stu-id="1dff0-209">For now, if you need tooarchive this data, you can download hello reports periodically and save them in a separate location.</span></span>
  >
  >
* <span data-ttu-id="1dff0-210">**K: van egy hello jelszó jelentések megjeleníthető sorok maximális számát?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-210">**Q:  Is there a maximum number of rows that can appear on hello password management reports?**</span></span>

  > <span data-ttu-id="1dff0-211">**V:** Igen, a 75,000 sorok maximális jelenhetnek meg akár hello jelszó jelentések, hogy azok alatt jelennek meg hello felhasználói felületén vagy a letöltött.</span><span class="sxs-lookup"><span data-stu-id="1dff0-211">**A:** Yes, a maximum of 75,000 rows may appear on either of hello password management reports, whether they are being shown in hello UI or being downloaded.</span></span>
  >
  >
* <span data-ttu-id="1dff0-212">**K: van egy API tooaccess hello jelszó alaphelyzetbe állítása vagy a regisztráció jelentési adatot?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-212">**Q:  Is there an API tooaccess hello password reset or registration reporting data?**</span></span>

  > <span data-ttu-id="1dff0-213">**V:** Igen, tekintse meg a hello következő dokumentáció toolearn hogyan férhet hozzá a hello jelszó alaphelyzetbe állítása jelentéskészítési adatfolyamban.</span><span class="sxs-lookup"><span data-stu-id="1dff0-213">**A:** Yes, see hello following documentation toolearn how you can access hello password reset reporting data stream.</span></span>  <span data-ttu-id="1dff0-214">[Ismerje meg, hogyan tooaccess jelszó-átállítási jelentési eseményeket programozott módon](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).</span><span class="sxs-lookup"><span data-stu-id="1dff0-214">[Learn how tooaccess password reset reporting events programmatically](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).</span></span>
  >
  >

## <a name="password-writeback"></a><span data-ttu-id="1dff0-215">Jelszóvisszaíró</span><span class="sxs-lookup"><span data-stu-id="1dff0-215">Password writeback</span></span>
* <span data-ttu-id="1dff0-216">**K: hogyan működik a jelszóvisszaírás hello háttérben?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-216">**Q:  How does password writeback work behind hello scenes?**</span></span>

  > <span data-ttu-id="1dff0-217">**V:** lásd [jelszóvisszaírás működése](active-directory-passwords-writeback.md) az annak magyarázatát, mi történik, ha engedélyezi a jelszóvisszaírás, és hogyan adatáramlás hello rendszeren keresztül vissza a helyszíni környezetbe.</span><span class="sxs-lookup"><span data-stu-id="1dff0-217">**A:** See [How password writeback works](active-directory-passwords-writeback.md) for an explanation of what happens when you enable password writeback, and how data flows through hello system back into your on-premises environment.</span></span>
  >
  >
* <span data-ttu-id="1dff0-218">**K: mennyi ideig tart a jelszóvisszaírás toowork?  A szinkronizálás késleltetés, például a Jelszókivonat-szinkronizálás van?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-218">**Q:  How long does password writeback take toowork?  Is there a synchronization delay like with password hash sync?**</span></span>

  > <span data-ttu-id="1dff0-219">**V:** jelszóvisszaírás azonnali.</span><span class="sxs-lookup"><span data-stu-id="1dff0-219">**A:** Password writeback is instant.</span></span> <span data-ttu-id="1dff0-220">Egy szinkron folyamatot, amely a Jelszókivonat-szinkronizálást alapvetően eltérően működik.</span><span class="sxs-lookup"><span data-stu-id="1dff0-220">It is a synchronous pipeline that works fundamentally differently than password hash synchronization.</span></span> <span data-ttu-id="1dff0-221">A jelszóvisszaírás engedélyezése a felhasználók tooget valós idejű visszajelzést hello sikeres, a jelszó alaphelyzetbe állítása, vagy módosítsa a műveletet.</span><span class="sxs-lookup"><span data-stu-id="1dff0-221">Password writeback allows users tooget real-time feedback about hello success of their password reset or change operation.</span></span> <span data-ttu-id="1dff0-222">hello átlagos jelszó sikeres visszaírása ideje alatt 500 ms.</span><span class="sxs-lookup"><span data-stu-id="1dff0-222">hello average time for a successful writeback of a password is under 500 ms.</span></span>
  >
  >
* <span data-ttu-id="1dff0-223">**K:, ha a helyi fiók le van tiltva, milyen hatással a van a felhő fiókelérést?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-223">**Q:  If my on-premises account is disabled, how is my cloud account/access affected?**</span></span>

  > <span data-ttu-id="1dff0-224">**V:** a helyszíni-azonosítója le van tiltva, ha a felhő azonosítója/hozzáférést is le lesz tiltva hello tovább szinkronizálás gyakorisága byt alapértelmezett AAD-csatlakozás keresztül ez az 30 percenként.</span><span class="sxs-lookup"><span data-stu-id="1dff0-224">**A:** If your on-premises ID is disabled, your cloud ID/access will also be disabled at hello next sync interval via AAD Connect byt default this is every 30 minutes.</span></span>
  >
  >
* <span data-ttu-id="1dff0-225">**K:, ha egy a helyszíni Active Directory jelszóházirend korlátozza a helyi fiók, önkiszolgáló jelszó-Változtatási felel meg a házirend hello jelszó módosításakor?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-225">**Q:  If my on-premises account is constrained by an on-premises Active Directory password policy, does SSPR obey this policy when I change hello password?**</span></span>

  > <span data-ttu-id="1dff0-226">**V:** Igen, önkiszolgáló jelszó-Változtatási alapul, és aláveti hello a helyszíni AD-jelszóházirendet, beleértve a tipikus AD tartományi jelszóházirend, valamint bármely meghatározott részletes jelszóházirendek felhasználói tooa céloz.</span><span class="sxs-lookup"><span data-stu-id="1dff0-226">**A:** Yes, SSPR relies on and abides by hello on-premises AD password policy, including typical AD domain password policy, as well as any defined fine grained password policies targeted tooa given user.</span></span>
  >
  >
* <span data-ttu-id="1dff0-227">**K: milyen típusú fiókok jelszóvisszaírás működik?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-227">**Q:  What types of accounts does password writeback work for?**</span></span>

  > <span data-ttu-id="1dff0-228">**V:** jelszóvisszaírás külső és a jelszó kivonatát szinkronizált felhasználók működik.</span><span class="sxs-lookup"><span data-stu-id="1dff0-228">**A:** Password writeback works for Federated and Password Hash Synchronized users.</span></span>
  >
  >
* <span data-ttu-id="1dff0-229">**K: jelszóvisszaírás jelszóházirendjeinek érvényesítéséhez a tartományomat?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-229">**Q:  Does password writeback enforce my domain’s password policies?**</span></span>

  > <span data-ttu-id="1dff0-230">**V:** Igen, érvénybe lépteti a jelszóvisszaírást a jelszó élettartama, előzmények, összetettségét, szűrők és helyezhet jelszavak helyen vannak a helyi tartományban lévő bármely más korlátozás.</span><span class="sxs-lookup"><span data-stu-id="1dff0-230">**A:** Yes, password writeback enforces password age, history, complexity, filters, and any other restriction you may put in place on passwords in your local domain.</span></span>
  >
  >
* <span data-ttu-id="1dff0-231">**K: az jelszóvisszaírás biztonságos?  Hogyan lehet, hogy I nem fognak első megtámadott?**</span><span class="sxs-lookup"><span data-stu-id="1dff0-231">**Q:  Is password writeback secure?  How can I be sure I won’t get hacked?**</span></span>

  > <span data-ttu-id="1dff0-232">**V:** Igen, a jelszóvisszaírás biztonságos-e.</span><span class="sxs-lookup"><span data-stu-id="1dff0-232">**A:** Yes, password writeback is secure.</span></span> <span data-ttu-id="1dff0-233">További információ a négy hello biztonsági réteg alkalmazása tooread hello jelszó visszaírási szolgáltatás által megvalósított, tekintse meg a hello [jelszó visszaírási biztonsági modell](active-directory-passwords-writeback.md#password-writeback-security-model) jelszóvisszaírás működése című szakasza.</span><span class="sxs-lookup"><span data-stu-id="1dff0-233">tooread more about hello four layers of security implemented by hello password writeback service, check out hello [Password writeback security model](active-directory-passwords-writeback.md#password-writeback-security-model) section in How password writeback works.</span></span>
  >
  >

## <a name="next-steps"></a><span data-ttu-id="1dff0-234">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1dff0-234">Next steps</span></span>

<span data-ttu-id="1dff0-235">a következő hivatkozások hello adja meg a jelszó alaphelyzetbe állítása, az Azure AD használatával kapcsolatos további információk</span><span class="sxs-lookup"><span data-stu-id="1dff0-235">hello following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="1dff0-236">[**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét.</span><span class="sxs-lookup"><span data-stu-id="1dff0-236">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="1dff0-237">[**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="1dff0-237">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="1dff0-238">[**Adatok** ](active-directory-passwords-data.md) - szükséges hello adatok megismeréséhez, és hogyan használja fel azokat a jelszókezelés</span><span class="sxs-lookup"><span data-stu-id="1dff0-238">[**Data**](active-directory-passwords-data.md) - Understand hello data that is required and how it is used for password management</span></span>
* <span data-ttu-id="1dff0-239">[**Bevezetés** ](active-directory-passwords-best-practices.md) -megtervezése és telepítése az önkiszolgáló jelszó-Változtatási tooyour felhasználók hello útmutatást itt talál</span><span class="sxs-lookup"><span data-stu-id="1dff0-239">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR tooyour users using hello guidance found here</span></span>
* <span data-ttu-id="1dff0-240">[**Testre szabhatja** ](active-directory-passwords-customize.md) -testreszabása, önkiszolgáló jelszó-Változtatási élményt a vállalata hello hello megjelenését és működését.</span><span class="sxs-lookup"><span data-stu-id="1dff0-240">[**Customize**](active-directory-passwords-customize.md) - Customize hello look and feel of hello SSPR experience for your company.</span></span>
* <span data-ttu-id="1dff0-241">[**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.</span><span class="sxs-lookup"><span data-stu-id="1dff0-241">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="1dff0-242">[**Szabályzat**](active-directory-passwords-policy.md) – Megismerheti és beállíthatja az Azure AD jelszószabályzatait.</span><span class="sxs-lookup"><span data-stu-id="1dff0-242">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="1dff0-243">[**Jelszóvisszaíró**](active-directory-passwords-writeback.md) – Megtudhatja, hogyan használhatja a jelszóvisszaírót a helyszíni címtárával.</span><span class="sxs-lookup"><span data-stu-id="1dff0-243">[**Password Writeback**](active-directory-passwords-writeback.md) - How does password writeback work with your on-premises directory</span></span>
* <span data-ttu-id="1dff0-244">[**Műszaki mélyreható** ](active-directory-passwords-how-it-works.md) -mögött hello függöny toounderstand nyissa meg annak működéséről</span><span class="sxs-lookup"><span data-stu-id="1dff0-244">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind hello curtain toounderstand how it works</span></span>
* <span data-ttu-id="1dff0-245">[**Hibaelhárítás** ](active-directory-passwords-troubleshoot.md) -megtudhatja, hogyan tooresolve közös állít ki, hogy az önkiszolgáló jelszó-Változtatási látható</span><span class="sxs-lookup"><span data-stu-id="1dff0-245">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how tooresolve common issues that we see with SSPR</span></span>
