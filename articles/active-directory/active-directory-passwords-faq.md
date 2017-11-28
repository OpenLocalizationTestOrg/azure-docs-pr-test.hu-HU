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
ms.openlocfilehash: b3fab99ff9fab5bc67fa70113dc5b06fac775b09
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="password-management-frequently-asked-questions"></a><span data-ttu-id="7438a-104">A jelszókezelés gyakran ismételt kérdések</span><span class="sxs-lookup"><span data-stu-id="7438a-104">Password management frequently asked questions</span></span>

<span data-ttu-id="7438a-105">Az alábbiakban néhány gyakori kérdés a jelszó alaphelyzetbe állítása kapcsolódó van.</span><span class="sxs-lookup"><span data-stu-id="7438a-105">The following are some frequently asked questions for all things related to password reset.</span></span>

<span data-ttu-id="7438a-106">Ha az Azure AD általános kérdése van, és az önkiszolgáló jelszó alaphelyzetbe állítása, nem megválaszolandó itt, megkérheti a közösségi Ha segítségre van szüksége a a [Azure Ad-fórumok](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD).</span><span class="sxs-lookup"><span data-stu-id="7438a-106">If you have a general question about Azure AD and self-service password reset, that is not answered here, you can ask the community for assistance on the [Azure Ad forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WindowsAzureAD).</span></span> <span data-ttu-id="7438a-107">A Közösség tagjai közé tartozik a mérnökök, termék kezelők, MVP és ösztöndíjas informatikai szakemberek számára.</span><span class="sxs-lookup"><span data-stu-id="7438a-107">Members of the community include Engineers, Product Managers, MVPs, and fellow IT Professionals.</span></span>

<span data-ttu-id="7438a-108">Ez a GYIK a következő szakaszok oszlik:</span><span class="sxs-lookup"><span data-stu-id="7438a-108">This FAQ is split into the following sections:</span></span>

* [<span data-ttu-id="7438a-109">**A Jelszóátállítás regisztrációját kapcsolatos kérdések**</span><span class="sxs-lookup"><span data-stu-id="7438a-109">**Questions about Password Reset Registration**</span></span>](#password-reset-registration)
* [<span data-ttu-id="7438a-110">**Jelszó alaphelyzetbe állítása kérdések**</span><span class="sxs-lookup"><span data-stu-id="7438a-110">**Questions about Password Reset**</span></span>](#password-reset)
* [<span data-ttu-id="7438a-111">**A jelszó módosítása kapcsolatos kérdések**</span><span class="sxs-lookup"><span data-stu-id="7438a-111">**Questions about Password Change**</span></span>](#password-change)
* [<span data-ttu-id="7438a-112">**Tudnivalók a jelszókezelésről kérdések jelentések**</span><span class="sxs-lookup"><span data-stu-id="7438a-112">**Questions about password management Reports**</span></span>](#password-management-reports)
* [<span data-ttu-id="7438a-113">**A jelszóvisszaírás kapcsolatos kérdések**</span><span class="sxs-lookup"><span data-stu-id="7438a-113">**Questions about password writeback**</span></span>](#password-writeback)

## <a name="password-reset-registration"></a><span data-ttu-id="7438a-114">Jelszó-átállítási regisztrációk</span><span class="sxs-lookup"><span data-stu-id="7438a-114">Password reset registration</span></span>
* <span data-ttu-id="7438a-115">**K: a felhasználók regisztrálhatják saját jelszó alaphelyzetbe állítása adatokat?**</span><span class="sxs-lookup"><span data-stu-id="7438a-115">**Q:  Can my users register their own password reset data?**</span></span>

  > <span data-ttu-id="7438a-116">**V:** Igen, mindaddig, amíg engedélyezve van a jelszó alaphelyzetbe állítása, és azok licencét, akkor lépjen a jelszó-átállítási regisztráció portálon, a hitelesítési adataikat regisztrálni http://aka.ms/ssprsetup.</span><span class="sxs-lookup"><span data-stu-id="7438a-116">**A:** Yes, as long as password reset is enabled and they are licensed, they can go to the Password Reset Registration portal at http://aka.ms/ssprsetup to register their authentication information.</span></span> <span data-ttu-id="7438a-117">A felhasználók is regisztrálhatják a hozzáférési panelre a http://myapps.microsoft.com címen, a profil lapon majd beállítás jelszó-átállítási regisztráció gombra kattintva.</span><span class="sxs-lookup"><span data-stu-id="7438a-117">Users can also register by going to the access panel at http://myapps.microsoft.com, clicking the profile tab, and clicking the Register for Password Reset option.</span></span>
  >
  >
* <span data-ttu-id="7438a-118">**K: jelszó alaphelyzetbe állítása adatok is definiálása a felhasználók nevében?**</span><span class="sxs-lookup"><span data-stu-id="7438a-118">**Q:  Can I define password reset data on behalf of my users?**</span></span>

  > <span data-ttu-id="7438a-119">**V:** Igen, akkor az Azure AD Connect, PowerShell, a [Azure-portálon](https://portal.azure.com), vagy az Office felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="7438a-119">**A:** Yes, you can do so with Azure AD Connect, PowerShell, the [Azure portal](https://portal.azure.com), or the Office Admin portal.</span></span> <span data-ttu-id="7438a-120">További információkért lásd: a cikk [az Azure AD önkiszolgáló jelszó-változtatási által használt adatok](active-directory-passwords-data.md).</span><span class="sxs-lookup"><span data-stu-id="7438a-120">For more information, see the article [Data used by Azure AD Self-Service Password Reset](active-directory-passwords-data.md).</span></span>
  >
  >
* <span data-ttu-id="7438a-121">**K: biztonsági kérdéseket tesz fel a helyszíni adatok szinkronizálása?**</span><span class="sxs-lookup"><span data-stu-id="7438a-121">**Q:  Can I synchronize data for security questions from on premises?**</span></span>

  > <span data-ttu-id="7438a-122">**V:** erre nincs lehetőség ma.</span><span class="sxs-lookup"><span data-stu-id="7438a-122">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="7438a-123">**K: a felhasználók regisztrálhatják adatokat úgy, hogy más felhasználók nem láthatják ezeket az adatokat?**</span><span class="sxs-lookup"><span data-stu-id="7438a-123">**Q:  Can my users register data in such a way that other users cannot see this data?**</span></span>

  > <span data-ttu-id="7438a-124">**V:** Igen, ha a felhasználók regisztrálhatnak az adatokat, és a jelszó-változtatási regisztrációs portálra a Mentés személyes hitelesítési mezőkbe, amelyek csak a globális rendszergazdák és a felhasználó által látható.</span><span class="sxs-lookup"><span data-stu-id="7438a-124">**A:** Yes, when users register data using the Password Reset Registration Portal it is saved into private authentication fields that are only visible by Global Administrators and the user.</span></span>
    >
    > [!NOTE]
    > <span data-ttu-id="7438a-125">Ha egy **Azure rendszergazdai fiók** regisztrálja a hitelesítéshez használni kívánt telefonszámot is a telepítéskor a mobiltelefon mezőbe, és látható.</span><span class="sxs-lookup"><span data-stu-id="7438a-125">If an **Azure Administrator account** registers their authentication phone number it is also populated into the mobile phone field and is visible.</span></span>
    >
  >
  >
* <span data-ttu-id="7438a-126">**K: a felhasználók rendelkeznek regisztrálni kell a jelszó alaphelyzetbe állítása használatához?**</span><span class="sxs-lookup"><span data-stu-id="7438a-126">**Q:  Do my users have to be registered before they can use password reset?**</span></span>

  > <span data-ttu-id="7438a-127">**V:** nem, a nevében elég hitelesítési adatainak megadása esetén felhasználóknak nem kell regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="7438a-127">**A:** No, if you define enough authentication information on their behalf, users do not have to register.</span></span> <span data-ttu-id="7438a-128">Jelszó-átállítási működik, amíg a megfelelően formázott a megfelelő mezőket a könyvtárban tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="7438a-128">Password reset works as long as you have properly formatted data stored in the appropriate fields in the directory.</span></span>
  >
  >
* <span data-ttu-id="7438a-129">**K: I szinkronizálása vagy a hitelesítéshez megadott telefonját, a hitelesítési e-mailben vagy alternatív hitelesítéshez megadott telefonját mezők beállítása a felhasználók nevében?**</span><span class="sxs-lookup"><span data-stu-id="7438a-129">**Q:  Can I synchronize or set the Authentication Phone, Authentication Email or Alternate Authentication Phone fields on behalf of my users?**</span></span>

  > <span data-ttu-id="7438a-130">**V:** erre nincs lehetőség ma.</span><span class="sxs-lookup"><span data-stu-id="7438a-130">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="7438a-131">**K: hogyan nem a regisztrációs portálon, hogy mely beállítások megjelenítése a felhasználók számára?**</span><span class="sxs-lookup"><span data-stu-id="7438a-131">**Q:  How does the registration portal know which options to show my users?**</span></span>

  > <span data-ttu-id="7438a-132">**V:** a jelszó-visszaállítási portál csak lehetőségeket mutatja be, hogy a felhasználók számára engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="7438a-132">**A:** The password reset registration portal only shows the options that you have enabled for your users.</span></span> <span data-ttu-id="7438a-133">Ezek a beállítások a könyvtár konfigurálása lap felhasználói jelszó-visszaállítási házirend szakasza alatt találhatók.</span><span class="sxs-lookup"><span data-stu-id="7438a-133">These options are found under the User Password Reset Policy section of your directory’s Configure tab.</span></span> <span data-ttu-id="7438a-134">Például ez azt jelenti, hogy ha nem engedélyezi a biztonsági kérdéseket, majd felhasználók képesek nem regisztrálja az adott beállítási mód.</span><span class="sxs-lookup"><span data-stu-id="7438a-134">For example, this means that if you do not enable security questions, then users are not able to register for that option.</span></span>
  >
  >
* <span data-ttu-id="7438a-135">**Amikor egy felhasználó tekinthető k: regisztrálni?**</span><span class="sxs-lookup"><span data-stu-id="7438a-135">**Q:  When is a user considered registered?**</span></span>

  > <span data-ttu-id="7438a-136">**V:** regisztrált sspr, amikor regisztrálják azokat A felhasználó akkor tekinthető legalább a **számos módszer alaphelyzetbe kell állítani** beállított a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7438a-136">**A:** A user is considered registered for SSPR when they have registered at least the **Number of methods required to reset** that you have set in the [Azure portal](https://portal.azure.com).</span></span>
  >
  >
## <a name="password-reset"></a><span data-ttu-id="7438a-137">Új jelszó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7438a-137">Password reset</span></span>
* <span data-ttu-id="7438a-138">**K: mennyi ideig várjon egy e-mailben, SMS vagy telefonhívás fogadása jelszó alaphelyzetbe állítása?**</span><span class="sxs-lookup"><span data-stu-id="7438a-138">**Q:  How long should I wait to receive an email, SMS, or phone call from password reset?**</span></span>

  > <span data-ttu-id="7438a-139">**V:** e-mailben, SMS-t, és telefonhívásokat egy perc alatt 5-20 másodperc normál esetben a kell érkeznek.</span><span class="sxs-lookup"><span data-stu-id="7438a-139">**A:** Email, SMS messages, and phone calls should arrive in under one minute, with the normal case being 5-20 seconds.</span></span>
    ><span data-ttu-id="7438a-140">Ha nem jelenik meg az értesítés a időkereten belül:</span><span class="sxs-lookup"><span data-stu-id="7438a-140">If you do not receive the notification in this time frame:</span></span>
        > * <span data-ttu-id="7438a-141">Ellenőrizze a Levélszemét mappát.</span><span class="sxs-lookup"><span data-stu-id="7438a-141">Check your junk folder.</span></span>
        > * <span data-ttu-id="7438a-142">Ellenőrizze a vagy e-mail, amelyhez csatlakozik egy várt.</span><span class="sxs-lookup"><span data-stu-id="7438a-142">Check the number or email being contacted is the one you expect.</span></span>
        > * <span data-ttu-id="7438a-143">Ellenőrizze, hogy a címtárban a hitelesítési adatok megfelelően van formázva.</span><span class="sxs-lookup"><span data-stu-id="7438a-143">Check the authentication data in the directory is correctly formatted.</span></span>
                >     * <span data-ttu-id="7438a-144">Példa: "+ 1 4255551234" vagy "user@contoso.com"</span><span class="sxs-lookup"><span data-stu-id="7438a-144">Example: "+1 4255551234" or "user@contoso.com"</span></span>
  >
  >
* <span data-ttu-id="7438a-145">**K: jelszó alaphelyzetbe állítása milyen nyelveket támogatja?**</span><span class="sxs-lookup"><span data-stu-id="7438a-145">**Q:  What languages are supported by password reset?**</span></span>

  > <span data-ttu-id="7438a-146">**V:** a jelszó-visszaállítási felhasználói felület, SMS-t, és hanghívások honosítva vannak az Office 365-ben támogatott nyelven.</span><span class="sxs-lookup"><span data-stu-id="7438a-146">**A:** The password reset UI, SMS messages, and voice calls are localized in the same languages that are supported in Office 365.</span></span>
  >
  >
* <span data-ttu-id="7438a-147">**A jelszó alaphelyzetbe állítása élmény részeinek első vállalati arculattal, szervezeti a címtárban branding meg k: tartozó konfigurálása lapon?**</span><span class="sxs-lookup"><span data-stu-id="7438a-147">**Q:  What parts of the password reset experience get branded when I set organizational branding in my directory’s configure tab?**</span></span>

  > <span data-ttu-id="7438a-148">**V:** a jelszó-változtatási portál jeleníti meg a szervezeti embléma, és lehetővé teszi az ügyfél konfigurálása a rendszergazda hivatkozás egy egyéni e-mail vagy URL-CÍMRE mutasson.</span><span class="sxs-lookup"><span data-stu-id="7438a-148">**A:** The password reset portal shows your organizational logo and allows you to configure the Contact your administrator link to point to a custom email or URL.</span></span> <span data-ttu-id="7438a-149">Minden e-mailben küldi el jelszóvisszaállítás tartalmazza a vállalati embléma, a színeket, a neve az e-mail törzsében, és testre szabott neve.</span><span class="sxs-lookup"><span data-stu-id="7438a-149">Any email that gets sent by password reset includes your organization’s logo, colors, name in the body of the email, and customized from name.</span></span>
  >
  >
* <span data-ttu-id="7438a-150">**K: hogyan lehet ismertetni kell a felhasználók visszaállíthassák a jelszavukat önállóan hol találhatnak kapcsolatban?**</span><span class="sxs-lookup"><span data-stu-id="7438a-150">**Q:  How can I educate my users about where to go to reset their passwords?**</span></span>

  > <span data-ttu-id="7438a-151">**V:** küldhet a felhasználók https://passwordreset.microsoftonline.com közvetlenül, vagy utasíthatja őket, kattintson a **nem tudja elérni a fiókot hivatkozás** bármely munkahelyi vagy iskolai bejelentkezési lapján található.</span><span class="sxs-lookup"><span data-stu-id="7438a-151">**A:** You can send your users to https://passwordreset.microsoftonline.com directly, or you can instruct them to click the **Can’t access your account link** found on any Work or School sign-in page.</span></span> <span data-ttu-id="7438a-152">A felhasználók számára könnyen hozzáférhető helyen is közzéteheti ezeket a hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="7438a-152">You can also publish these links in a place easily accessible to your users.</span></span>
  >
  >
* <span data-ttu-id="7438a-153">**K: használhatok ezen a lapon egy mobileszközről?**</span><span class="sxs-lookup"><span data-stu-id="7438a-153">**Q:  Can I use this page from a mobile device?**</span></span>

  > <span data-ttu-id="7438a-154">**V:** Igen, az ezen a lapon a mobileszközök működik.</span><span class="sxs-lookup"><span data-stu-id="7438a-154">**A:** Yes, this page works on mobile devices.</span></span>
  >
  >
* <span data-ttu-id="7438a-155">**K: támogatják a feloldásának helyi active directory-fiókokat, amikor a felhasználók visszaállíthassák a jelszavukat?**</span><span class="sxs-lookup"><span data-stu-id="7438a-155">**Q:  Do you support unlocking local active directory accounts when users reset their passwords?**</span></span>

  > <span data-ttu-id="7438a-156">**V:** Igen, ha a felhasználó visszaállítja a jelszavát, és a jelszóvisszaíró az Azure AD Connect használatával lett telepítve, adott felhasználói fiók esetén automatikusan feloldani a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="7438a-156">**A:** Yes, when a user resets their password and password writeback has been deployed using Azure AD Connect, that user’s account is automatically unlocked when they reset their password.</span></span>
  >
  >
* <span data-ttu-id="7438a-157">**K: hogyan integrálhatók a jelszó alaphelyzetbe állítása közvetlenül a saját felhasználói asztali bejelentkezés során tapasztal élmény?**</span><span class="sxs-lookup"><span data-stu-id="7438a-157">**Q:  How can I integrate password reset directly into my user’s desktop sign-in experience?**</span></span>

  > <span data-ttu-id="7438a-158">**V:** Ha az Azure AD Premium felhasználóinál, telepítse a Microsoft Identity Manager további költségek nélkül, és ez a követelmény teljesítéséhez a helyszíni jelszó alaphelyzetbe állítása megoldás üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="7438a-158">**A:** If you are an Azure AD Premium customer, you can install Microsoft Identity Manager at no additional cost and deploy the on-premises password reset solution to meet this requirement.</span></span>
  >
  >
* <span data-ttu-id="7438a-159">**K: beállítása a különböző területi beállításokhoz különböző biztonsági kérdést?**</span><span class="sxs-lookup"><span data-stu-id="7438a-159">**Q:  Can I set different security questions for different locales?**</span></span>

  > <span data-ttu-id="7438a-160">**V:** erre nincs lehetőség ma.</span><span class="sxs-lookup"><span data-stu-id="7438a-160">**A:** This is not possible today.</span></span>
  >
  >
* <span data-ttu-id="7438a-161">**K: hogyan több kérdést azt a állíthatja be a biztonsági kérdések hitelesítési lehetőséget?**</span><span class="sxs-lookup"><span data-stu-id="7438a-161">**Q:  How many questions can we configure for the Security Questions authentication option?**</span></span>

  > <span data-ttu-id="7438a-162">**V:** legfeljebb 20 egyéni biztonsági kérdéseket is beállíthat a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7438a-162">**A:** You can configure up to 20 custom security questions in the [Azure portal](https://portal.azure.com).</span></span>
  >
  >
* <span data-ttu-id="7438a-163">**K: mennyi ideig lehet, hogy a biztonsági kérdések kell?**</span><span class="sxs-lookup"><span data-stu-id="7438a-163">**Q:  How long may security questions be?**</span></span>

  > <span data-ttu-id="7438a-164">**V:** biztonsági kérdések 3 és 200 karakter közötti lehet.</span><span class="sxs-lookup"><span data-stu-id="7438a-164">**A:** Security questions may be between 3 and 200 characters long.</span></span>
  >
  >
* <span data-ttu-id="7438a-165">**K: mennyi ideig biztonsági kérdésekre adott válaszok lehet?**</span><span class="sxs-lookup"><span data-stu-id="7438a-165">**Q:  How long may answers to security questions be?**</span></span>

  > <span data-ttu-id="7438a-166">**V:** válaszok 3-40 karakter hosszú lehet.</span><span class="sxs-lookup"><span data-stu-id="7438a-166">**A:** Answers may be 3 to 40 characters long.</span></span>
  >
  >
* <span data-ttu-id="7438a-167">**K: ismétlődő válaszokat ad a biztonsági kérdésekre elutasított vannak?**</span><span class="sxs-lookup"><span data-stu-id="7438a-167">**Q:  Are duplicate answers to security questions rejected?**</span></span>

  > <span data-ttu-id="7438a-168">**V:** Igen, azt utasítsa el az ismétlődő válaszokat ad a biztonsági kérdésekre.</span><span class="sxs-lookup"><span data-stu-id="7438a-168">**A:** Yes, we reject duplicate answers to security questions.</span></span>
  >
  >
* <span data-ttu-id="7438a-169">**K: Előfordulhat, hogy a felhasználó regisztrálása a ugyanazt biztonsági kérdésre adott egynél többször?**</span><span class="sxs-lookup"><span data-stu-id="7438a-169">**Q:  May a user register the same security question more than once?**</span></span>

  > <span data-ttu-id="7438a-170">**V:** nem, miután a felhasználó regisztrál egy adott kérdést, előfordulhat, hogy nem regisztrálják az adott kérdést, még egyszer.</span><span class="sxs-lookup"><span data-stu-id="7438a-170">**A:** No, once a user registers a particular question, they may not register for that question a second time.</span></span>
  >
  >
* <span data-ttu-id="7438a-171">**K: van lehetőség a minimális addig, amíg a regisztráció biztonsági kérdések, és alaphelyzetbe állítja?**</span><span class="sxs-lookup"><span data-stu-id="7438a-171">**Q:  Is it possible to set a minimum limit of security questions for registration and reset?**</span></span>

  > <span data-ttu-id="7438a-172">**V:** Igen, egy korlát a regisztrációs és egy másik alaphelyzetbe állítása a állítható be.</span><span class="sxs-lookup"><span data-stu-id="7438a-172">**A:** Yes, one limit can be set for registration and another for reset.</span></span> <span data-ttu-id="7438a-173">lehet, hogy 3-5 biztonsági kérdések regisztrációjához szükséges, és lehet, hogy 3-5 alaphelyzetbe állítása szükséges.</span><span class="sxs-lookup"><span data-stu-id="7438a-173">3-5 security questions may be required for registration and 3-5 may be required for reset.</span></span>
  >
  >
* <span data-ttu-id="7438a-174">**K:, ha a felhasználó regisztrálva van a több maximális kérdések alaphelyzetbe kell állítani, hogyan vannak a biztonsági kérdések kijelölt alaphelyzetbe állítása során?**</span><span class="sxs-lookup"><span data-stu-id="7438a-174">**Q:  If a user has registered more than the maximum number of questions required to reset, how are security questions selected during reset?**</span></span>

  > <span data-ttu-id="7438a-175">**V:** N biztonsági kérdések véletlenszerűen kiválasztott felhasználó regisztrálva van, ahol N az kérdések száma kívül a **alaphelyzetbe kell állítani kérdések számát**.</span><span class="sxs-lookup"><span data-stu-id="7438a-175">**A:** N security questions are selected at random out of the total number of questions a user has registered for, where N is the **Number of questions required to reset**.</span></span> <span data-ttu-id="7438a-176">Például ha egy felhasználó rendelkezik-e regisztrálva 5 biztonsági kérdéseit, de csak 3 szükséges alaphelyzetbe állítása, az 5 3 véletlenszerűen és jelenik meg a következő alaphelyzetbe állítása.</span><span class="sxs-lookup"><span data-stu-id="7438a-176">For example, if a user has 5 security questions registered, but only 3 are required to reset, 3 of the 5 are selected randomly and presented at reset.</span></span> <span data-ttu-id="7438a-177">Ha a felhasználó élvezheti a kérdésekre adott válaszai nem megfelelő, a tanúsítványkiválasztási folyamat nem szűnik meg kérdés beütés megelőzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="7438a-177">If the user gets the answers to the questions wrong, the selection process reoccurs to prevent question hammering.</span></span>
  >
  >
* <span data-ttu-id="7438a-178">**K: el azt, hogy felhasználók jelszó-változtatási rövid időn belül időn belül többször próbálnak?**</span><span class="sxs-lookup"><span data-stu-id="7438a-178">**Q:  Do you prevent users from attempting password reset many times in a short time period?**</span></span>

  > <span data-ttu-id="7438a-179">**V:** Igen, vannak beépítve a jelszó alaphelyzetbe állítása visszaélés elleni védelme érdekében biztonsági funkciók.</span><span class="sxs-lookup"><span data-stu-id="7438a-179">**A:** Yes, there are security features built into password reset to protect from misuse.</span></span> <span data-ttu-id="7438a-180">Előfordulhat, hogy a felhasználók csak megpróbálják 5 jelszó alaphelyzetbe állítása kísérletek 24 óra alatt zárolása előtt egy órán belül.</span><span class="sxs-lookup"><span data-stu-id="7438a-180">Users may only try 5 password reset attempts within an hour before being locked out for 24 hours.</span></span> <span data-ttu-id="7438a-181">Felhasználók csak próbálkozzon 5 alkalommal 24 óra alatt zárolása előtt egy órán belül egy telefonszámot érvényesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7438a-181">Users may only try to validate a phone number 5 times within an hour before being locked out for 24 hours.</span></span> <span data-ttu-id="7438a-182">A felhasználók csak lehet, hogy megpróbálják egy egyetlen hitelesítési módszer 24 óra alatt zárolása előtt egy órán belül 5 alkalommal.</span><span class="sxs-lookup"><span data-stu-id="7438a-182">Users may only try a single authentication method 5 times within an hour before being locked out for 24 hours.</span></span>
  >
  >
* <span data-ttu-id="7438a-183">**K:, hogy mennyi ideig érvényesek az e-mailek és SMS egyszer használatos jelszót?**</span><span class="sxs-lookup"><span data-stu-id="7438a-183">**Q:  For how long are the email and SMS one-time passcode valid?**</span></span>

  > <span data-ttu-id="7438a-184">**V:** a munkamenetek élettartamát, jelszó-visszaállításhoz érték 105 perc.</span><span class="sxs-lookup"><span data-stu-id="7438a-184">**A:** The session lifetime for password reset is 105 minutes.</span></span> <span data-ttu-id="7438a-185">A jelszó-visszaállítási művelet kezdetétől a felhasználó rendelkezik-e jelszavuk 105 perc.</span><span class="sxs-lookup"><span data-stu-id="7438a-185">From the beginning of the password reset operation, the user has 105 minutes to reset their password.</span></span> <span data-ttu-id="7438a-186">Az e-mailek és SMS egyszer használatos jelszót érvénytelenek, ebben az időszakban lejárata után is.</span><span class="sxs-lookup"><span data-stu-id="7438a-186">The email and SMS one-time passcode are invalid after this time period expires.</span></span>
  >
  >

## <a name="password-change"></a><span data-ttu-id="7438a-187">A jelszó módosítása</span><span class="sxs-lookup"><span data-stu-id="7438a-187">Password change</span></span>
* <span data-ttu-id="7438a-188">**K: hol kell nyissa meg a felhasználók a jelszavuk módosítása?**</span><span class="sxs-lookup"><span data-stu-id="7438a-188">**Q:  Where should my users go to change their passwords?**</span></span>

  > <span data-ttu-id="7438a-189">**V:** felhasználók előfordulhat, hogy módosítsák jelszavukat bárhol akkor jelenik meg a profil kép vagy ikon (felső sarkában, például a [Office 365](https://portal.office.com) vagy [hozzáférési Panel](https://myapps.microsoft.com) észlel.</span><span class="sxs-lookup"><span data-stu-id="7438a-189">**A:** Users may change their passwords anywhere they see their profile picture or icon (like in the upper right corner of their [Office 365](https://portal.office.com) or [Access Panel](https://myapps.microsoft.com) experiences.</span></span> <span data-ttu-id="7438a-190">Felhasználók módosíthatja a jelszavukat a a [hozzáférési Panel profilszerkesztési lap](https://account.activedirectory.windowsazure.com/r#/profile).</span><span class="sxs-lookup"><span data-stu-id="7438a-190">Users may change their passwords from the [Access Panel profile page](https://account.activedirectory.windowsazure.com/r#/profile).</span></span> <span data-ttu-id="7438a-191">Felhasználók is kérheti a jelszavukat, automatikusan az Azure AD bejelentkezési képernyő, ha a jelszavuk.</span><span class="sxs-lookup"><span data-stu-id="7438a-191">Users may also be asked to change their passwords automatically at the Azure AD sign-in screen if their passwords have expired.</span></span> <span data-ttu-id="7438a-192">Végezetül felhasználók előfordulhat, hogy keresse meg a [Azure AD-jelszó módosítása Portal](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) közvetlenül történő jelszavukat.</span><span class="sxs-lookup"><span data-stu-id="7438a-192">Finally, users may navigate to the [Azure AD Password Change Portal](https://account.activedirectory.windowsazure.com/ChangePassword.aspx) directly if they wish to change their passwords.</span></span>
  >
  >
* <span data-ttu-id="7438a-193">**K: a felhasználók értesítést kaphat az Office portálon helyszíni jelszavukat lejárati?**</span><span class="sxs-lookup"><span data-stu-id="7438a-193">**Q:  Can my users be notified in the Office Portal when their on-premises password expires?**</span></span>

  > <span data-ttu-id="7438a-194">**V:** Ez azért lehetséges ma használata az AD FS utasítások itt: [küldése jelszó házirend jogcímek az AD FS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396).</span><span class="sxs-lookup"><span data-stu-id="7438a-194">**A:** This is possible today if you are using ADFS by following the instructions here: [Sending Password Policy Claims with ADFS](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configure-ad-fs-to-send-password-expiry-claims?f=255&MSPPError=-2147217396).</span></span> <span data-ttu-id="7438a-195">Ha a Jelszókivonat-szinkronizálást használ, ez nem lehetséges ma.</span><span class="sxs-lookup"><span data-stu-id="7438a-195">If you are using password hash synchronization, this is not possible today.</span></span> <span data-ttu-id="7438a-196">Ennek oka az, nem azt szinkronizálása a helyszíni jelszóházirendek, ezért nem lehetséges utáni lejárati értesítéseinek feladatait a felhőbe való.</span><span class="sxs-lookup"><span data-stu-id="7438a-196">This is because we do not sync password policies from on-premises, so it is not possible for us to post expiry notifications to cloud experiences.</span></span> <span data-ttu-id="7438a-197">Mindkét esetben is lehetőség [értesítse a felhasználókat, amelyeknek a jelszava lejár, a PowerShell használatával](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).</span><span class="sxs-lookup"><span data-stu-id="7438a-197">In either case, it is also possible to [notify users whose passwords are about to expire by using PowerShell](https://social.technet.microsoft.com/wiki/contents/articles/23313.notify-active-directory-users-about-password-expiry-using-powershell.aspx).</span></span>
  >
  >

## <a name="password-management-reports"></a><span data-ttu-id="7438a-198">Jelszó-kezelési jelentések</span><span class="sxs-lookup"><span data-stu-id="7438a-198">Password management reports</span></span>
* <span data-ttu-id="7438a-199">**K: mennyi ideig tart a jelenik meg a jelszó jelentések adatai?**</span><span class="sxs-lookup"><span data-stu-id="7438a-199">**Q:  How long does it take for data to show up on the password management reports?**</span></span>

  > <span data-ttu-id="7438a-200">**V:** adatokat meg kell jelennie a jelszó jelentések 5-10 percen belül.</span><span class="sxs-lookup"><span data-stu-id="7438a-200">**A:** Data should appear on the password management reports within 5-10 minutes.</span></span> <span data-ttu-id="7438a-201">Az egyes példányok, azt is tarthat egy órát jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="7438a-201">It some instances it may take up to an hour to appear.</span></span>
  >
  >
* <span data-ttu-id="7438a-202">**K: hogyan szűrheti a jelszó-kezelési jelentések**</span><span class="sxs-lookup"><span data-stu-id="7438a-202">**Q:  How can I filter the password management reports?**</span></span>

  > <span data-ttu-id="7438a-203">**V:** a kis nagyítóüveg szélsőséges jobb oldalán az oszlopfejléceket, a jelentés felső részén kattintson a jelszó jelentések szűrheti.</span><span class="sxs-lookup"><span data-stu-id="7438a-203">**A:** You can filter the password management reports by clicking the small magnifying glass to the extreme right of the column labels, near the top of the report.</span></span> <span data-ttu-id="7438a-204">Ha szeretné gazdagabb szűrés, letöltheti a jelentést az excel és kimutatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7438a-204">If you want to do richer filtering, you can download the report to excel and create a pivot table.</span></span>
  >
  >
* <span data-ttu-id="7438a-205">**K: Mi az az események maximális száma a jelszó jelentések tárolódnak?**</span><span class="sxs-lookup"><span data-stu-id="7438a-205">**Q: What is the maximum number of events are stored in the password management reports?**</span></span>

  > <span data-ttu-id="7438a-206">**V:** 75,000 jelszó alaphelyzetbe állítása vagy a jelszó alaphelyzetbe állítása regisztrációs események tárolódnak a jelszó-kezelési jelentések, akár átfedés biztonsági akár 30 napig.</span><span class="sxs-lookup"><span data-stu-id="7438a-206">**A:** Up to 75,000 password reset or password reset registration events are stored in the password management reports, spanning back up to 30 days.</span></span>  <span data-ttu-id="7438a-207">Jelenleg dolgozunk, bontsa ki ezt a számot, hogy további események tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="7438a-207">We are working to expand this number to include more events.</span></span>
  >
  >
* <span data-ttu-id="7438a-208">**K: milyen távolságban vissza a jelszó jelentések Ugrás?**</span><span class="sxs-lookup"><span data-stu-id="7438a-208">**Q:  How far back do the password management reports go?**</span></span>

  > <span data-ttu-id="7438a-209">**V:** a jelszókezelés jelentések megjelenítése művelet az elmúlt 30 napban.</span><span class="sxs-lookup"><span data-stu-id="7438a-209">**A:** The password management reports show operations occurring within the last 30 days.</span></span> <span data-ttu-id="7438a-210">A lépést Ha ezek az adatok archiválása kell után rendszeresen töltse le a jelentések is mentse őket másik helyen.</span><span class="sxs-lookup"><span data-stu-id="7438a-210">For now, if you need to archive this data, you can download the reports periodically and save them in a separate location.</span></span>
  >
  >
* <span data-ttu-id="7438a-211">**K: van-e a jelszó jelentések megjeleníthető sorok maximális száma?**</span><span class="sxs-lookup"><span data-stu-id="7438a-211">**Q:  Is there a maximum number of rows that can appear on the password management reports?**</span></span>

  > <span data-ttu-id="7438a-212">**V:** Igen, 75,000 sorok maximális szerepelhet vagy a jelszó jelentések, hogy éppen megtekinthető a felhasználói felületen vagy tölti le.</span><span class="sxs-lookup"><span data-stu-id="7438a-212">**A:** Yes, a maximum of 75,000 rows may appear on either of the password management reports, whether they are being shown in the UI or being downloaded.</span></span>
  >
  >
* <span data-ttu-id="7438a-213">**K: van egy API-t a jelszó alaphelyzetbe állítása vagy a jelentés adatainak regisztrációs eléréséhez?**</span><span class="sxs-lookup"><span data-stu-id="7438a-213">**Q:  Is there an API to access the password reset or registration reporting data?**</span></span>

  > <span data-ttu-id="7438a-214">**V:** Igen, tekintse meg a következő dokumentációjából megtudhatja, hogyan férhet hozzá a jelszó-változtatási reporting adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="7438a-214">**A:** Yes, see the following documentation to learn how you can access the password reset reporting data stream.</span></span>  <span data-ttu-id="7438a-215">[Jelszó alaphelyzetbe állítása programozott módon jelentési események elérése](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).</span><span class="sxs-lookup"><span data-stu-id="7438a-215">[Learn how to access password reset reporting events programmatically](https://msdn.microsoft.com/library/azure/mt126081.aspx#BKMK_SsprActivityEvent).</span></span>
  >
  >

## <a name="password-writeback"></a><span data-ttu-id="7438a-216">Jelszóvisszaíró</span><span class="sxs-lookup"><span data-stu-id="7438a-216">Password writeback</span></span>
* <span data-ttu-id="7438a-217">**K: hogyan működik a jelszóvisszaírást a háttérben?**</span><span class="sxs-lookup"><span data-stu-id="7438a-217">**Q:  How does password writeback work behind the scenes?**</span></span>

  > <span data-ttu-id="7438a-218">**V:** lásd [jelszóvisszaírás működése](active-directory-passwords-writeback.md) az annak magyarázatát, mi történik, ha engedélyezi a jelszóvisszaírás, és hogyan adatáramlás rendszeren keresztül vissza a helyszíni környezetbe.</span><span class="sxs-lookup"><span data-stu-id="7438a-218">**A:** See [How password writeback works](active-directory-passwords-writeback.md) for an explanation of what happens when you enable password writeback, and how data flows through the system back into your on-premises environment.</span></span>
  >
  >
* <span data-ttu-id="7438a-219">**K: mennyi időt jelszóvisszaírás igénybe működéséhez?  A szinkronizálás késleltetés, például a Jelszókivonat-szinkronizálás van?**</span><span class="sxs-lookup"><span data-stu-id="7438a-219">**Q:  How long does password writeback take to work?  Is there a synchronization delay like with password hash sync?**</span></span>

  > <span data-ttu-id="7438a-220">**V:** jelszóvisszaírás azonnali.</span><span class="sxs-lookup"><span data-stu-id="7438a-220">**A:** Password writeback is instant.</span></span> <span data-ttu-id="7438a-221">Egy szinkron folyamatot, amely a Jelszókivonat-szinkronizálást alapvetően eltérően működik.</span><span class="sxs-lookup"><span data-stu-id="7438a-221">It is a synchronous pipeline that works fundamentally differently than password hash synchronization.</span></span> <span data-ttu-id="7438a-222">A jelszóvisszaírás lehetővé teszi a felhasználóknak a valós idejű visszajelzést a jelszó alaphelyzetbe állítása sikeres vagy módosítsa a műveletet.</span><span class="sxs-lookup"><span data-stu-id="7438a-222">Password writeback allows users to get real-time feedback about the success of their password reset or change operation.</span></span> <span data-ttu-id="7438a-223">A jelszó sikeres visszaírása átlagos ideje alatt 500 ms.</span><span class="sxs-lookup"><span data-stu-id="7438a-223">The average time for a successful writeback of a password is under 500 ms.</span></span>
  >
  >
* <span data-ttu-id="7438a-224">**K:, ha a helyi fiók le van tiltva, milyen hatással a van a felhő fiókelérést?**</span><span class="sxs-lookup"><span data-stu-id="7438a-224">**Q:  If my on-premises account is disabled, how is my cloud account/access affected?**</span></span>

  > <span data-ttu-id="7438a-225">**V:** a helyszíni-azonosítója le van tiltva, ha a felhő azonosítója/access is le lesz tiltva a következő szinkronizálás időközönként byt alapértelmezett AAD-csatlakozás keresztül ez az 30 percenként.</span><span class="sxs-lookup"><span data-stu-id="7438a-225">**A:** If your on-premises ID is disabled, your cloud ID/access will also be disabled at the next sync interval via AAD Connect byt default this is every 30 minutes.</span></span>
  >
  >
* <span data-ttu-id="7438a-226">**K:, ha egy a helyszíni Active Directory jelszóházirend korlátozza a helyi fiók, önkiszolgáló jelszó-Változtatási felel meg a házirend a jelszó módosításakor?**</span><span class="sxs-lookup"><span data-stu-id="7438a-226">**Q:  If my on-premises account is constrained by an on-premises Active Directory password policy, does SSPR obey this policy when I change the password?**</span></span>

  > <span data-ttu-id="7438a-227">**V:** Igen, önkiszolgáló jelszó-Változtatási alapul, és megfelel a helyszíni AD-jelszóházirendet, beleértve az AD-tartományi jelszóházirend tipikus, valamint a meghatározott részletes jelszóházirendek egy adott felhasználót céloz meg.</span><span class="sxs-lookup"><span data-stu-id="7438a-227">**A:** Yes, SSPR relies on and abides by the on-premises AD password policy, including typical AD domain password policy, as well as any defined fine grained password policies targeted to a given user.</span></span>
  >
  >
* <span data-ttu-id="7438a-228">**K: milyen típusú fiókok jelszóvisszaírás működik?**</span><span class="sxs-lookup"><span data-stu-id="7438a-228">**Q:  What types of accounts does password writeback work for?**</span></span>

  > <span data-ttu-id="7438a-229">**V:** jelszóvisszaírás külső és a jelszó kivonatát szinkronizált felhasználók működik.</span><span class="sxs-lookup"><span data-stu-id="7438a-229">**A:** Password writeback works for Federated and Password Hash Synchronized users.</span></span>
  >
  >
* <span data-ttu-id="7438a-230">**K: jelszóvisszaírás jelszóházirendjeinek érvényesítéséhez a tartományomat?**</span><span class="sxs-lookup"><span data-stu-id="7438a-230">**Q:  Does password writeback enforce my domain’s password policies?**</span></span>

  > <span data-ttu-id="7438a-231">**V:** Igen, érvénybe lépteti a jelszóvisszaírást a jelszó élettartama, előzmények, összetettségét, szűrők és helyezhet jelszavak helyen vannak a helyi tartományban lévő bármely más korlátozás.</span><span class="sxs-lookup"><span data-stu-id="7438a-231">**A:** Yes, password writeback enforces password age, history, complexity, filters, and any other restriction you may put in place on passwords in your local domain.</span></span>
  >
  >
* <span data-ttu-id="7438a-232">**K: az jelszóvisszaírás biztonságos?  Hogyan lehet, hogy I nem fognak első megtámadott?**</span><span class="sxs-lookup"><span data-stu-id="7438a-232">**Q:  Is password writeback secure?  How can I be sure I won’t get hacked?**</span></span>

  > <span data-ttu-id="7438a-233">**V:** Igen, a jelszóvisszaírás biztonságos-e.</span><span class="sxs-lookup"><span data-stu-id="7438a-233">**A:** Yes, password writeback is secure.</span></span> <span data-ttu-id="7438a-234">Olvasható további információ a négy biztonsági réteg alkalmazása a jelszó visszaírási szolgáltatás által megvalósított, tekintse meg a [jelszó visszaírási biztonsági modell](active-directory-passwords-writeback.md#password-writeback-security-model) jelszóvisszaírás működése című szakasza.</span><span class="sxs-lookup"><span data-stu-id="7438a-234">To read more about the four layers of security implemented by the password writeback service, check out the [Password writeback security model](active-directory-passwords-writeback.md#password-writeback-security-model) section in How password writeback works.</span></span>
  >
  >

## <a name="next-steps"></a><span data-ttu-id="7438a-235">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7438a-235">Next steps</span></span>

<span data-ttu-id="7438a-236">Az alábbi hivatkozásokat követve az Azure AD jelszóátállításáról olvashat további információkat.</span><span class="sxs-lookup"><span data-stu-id="7438a-236">The following links provide additional information regarding password reset using Azure AD</span></span>

* <span data-ttu-id="7438a-237">[**Gyors üzembe helyezés**](active-directory-passwords-getting-started.md) – Percek alatt üzembe helyezheti az Azure AD önkiszolgáló jelszókezelőjét.</span><span class="sxs-lookup"><span data-stu-id="7438a-237">[**Quick Start**](active-directory-passwords-getting-started.md) - Get up and running with Azure AD self service password management</span></span> 
* <span data-ttu-id="7438a-238">[**Licencelés**](active-directory-passwords-licensing.md) – Az Azure AD licencelésének konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="7438a-238">[**Licensing**](active-directory-passwords-licensing.md) - Configure your Azure AD Licensing</span></span>
* <span data-ttu-id="7438a-239">[**Adatok**](active-directory-passwords-data.md) – A szükséges adatok megismerése, és az adatok használata a rendszer jelszókezelésre.</span><span class="sxs-lookup"><span data-stu-id="7438a-239">[**Data**](active-directory-passwords-data.md) - Understand the data that is required and how it is used for password management</span></span>
* <span data-ttu-id="7438a-240">[**Bevezetés**](active-directory-passwords-best-practices.md) – Az SSPR funkció tervezése és üzembe helyezése a felhasználók számára az itt található útmutató segítségével.</span><span class="sxs-lookup"><span data-stu-id="7438a-240">[**Rollout**](active-directory-passwords-best-practices.md) - Plan and deploy SSPR to your users using the guidance found here</span></span>
* <span data-ttu-id="7438a-241">[**Testreszabás**](active-directory-passwords-customize.md) – Az SSPR-felület megjelenésének és működésének testre szabása a cége számára.</span><span class="sxs-lookup"><span data-stu-id="7438a-241">[**Customize**](active-directory-passwords-customize.md) - Customize the look and feel of the SSPR experience for your company.</span></span>
* <span data-ttu-id="7438a-242">[**Jelentéskészítés**](active-directory-passwords-reporting.md) – Megtudhatja, mikor és hol érik el a felhasználói az SSPR funkcióit.</span><span class="sxs-lookup"><span data-stu-id="7438a-242">[**Reporting**](active-directory-passwords-reporting.md) - Discover if, when, and where your users are accessing SSPR functionality</span></span>
* <span data-ttu-id="7438a-243">[**Szabályzat**](active-directory-passwords-policy.md) – Megismerheti és beállíthatja az Azure AD jelszószabályzatait.</span><span class="sxs-lookup"><span data-stu-id="7438a-243">[**Policy**](active-directory-passwords-policy.md) - Understand and set Azure AD password policies</span></span>
* <span data-ttu-id="7438a-244">[**Jelszóvisszaíró**](active-directory-passwords-writeback.md) – Megtudhatja, hogyan használhatja a jelszóvisszaírót a helyszíni címtárával.</span><span class="sxs-lookup"><span data-stu-id="7438a-244">[**Password Writeback**](active-directory-passwords-writeback.md) - How does password writeback work with your on-premises directory</span></span>
* <span data-ttu-id="7438a-245">[**Részletes műszaki bemutatás**](active-directory-passwords-how-it-works.md) – Egy pillantás a függöny mögé, hogy megértse, hogyan is működik.</span><span class="sxs-lookup"><span data-stu-id="7438a-245">[**Technical Deep Dive**](active-directory-passwords-how-it-works.md) - Go behind the curtain to understand how it works</span></span>
* <span data-ttu-id="7438a-246">[**Hibaelhárítás**](active-directory-passwords-troubleshoot.md) – Ismerje meg, hogyan oldhat meg általános, az SSPR működése során jelentkező hibákat.</span><span class="sxs-lookup"><span data-stu-id="7438a-246">[**Troubleshoot**](active-directory-passwords-troubleshoot.md) - Learn how to resolve common issues that we see with SSPR</span></span>
