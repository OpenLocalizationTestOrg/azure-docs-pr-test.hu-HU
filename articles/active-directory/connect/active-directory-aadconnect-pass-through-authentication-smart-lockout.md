---
title: "Az Azure AD Connect: Áteresztő hitelesítés – intelligens zárolás |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan Azure Active Directory (Azure AD) áteresztő hitelesítés védelmet nyújt a helyszíni fiókok hello felhőben találgatásos jelszó támadásoktól."
services: active-directory
keywords: "Az Azure AD Connect áteresztő hitelesítés, telepítés Active Directory szükséges összetevőket az Azure AD, SSO, egyszeri bejelentkezést."
documentationcenter: 
author: swkrish
manager: femila
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: b02e315c3cc3eae00ca6408d735a416f34c2cdc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-pass-through-authentication-smart-lockout"></a><span data-ttu-id="1028f-104">Az Azure Active Directory áteresztő hitelesítés: Az intelligens zárolás</span><span class="sxs-lookup"><span data-stu-id="1028f-104">Azure Active Directory Pass-through Authentication: Smart Lockout</span></span>

## <a name="overview"></a><span data-ttu-id="1028f-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="1028f-105">Overview</span></span>

<span data-ttu-id="1028f-106">Az Azure AD találgatásos jelszó támadások ellen, és megakadályozza, hogy a valódi felhasználók az Office 365 és az SaaS-alkalmazások kívül zárás alatt.</span><span class="sxs-lookup"><span data-stu-id="1028f-106">Azure AD protects against brute force password attacks and prevents genuine users from being locked out of their Office 365 and SaaS applications.</span></span> <span data-ttu-id="1028f-107">Ez a funkció hívása **intelligens zárolás**, akkor támogatott, ha átmenő hitelesítést, a bejelentkezési módszer használatát.</span><span class="sxs-lookup"><span data-stu-id="1028f-107">This capability, called **Smart Lockout**, is supported when you use Pass-through Authentication as your sign-in method.</span></span> <span data-ttu-id="1028f-108">Intelligens zárolás alapértelmezés szerint engedélyezve van az összes bérlőre vonatkozó és védeni, a felhasználói fiókok minden hello idő; nincs szükség tooturn van azt meg.</span><span class="sxs-lookup"><span data-stu-id="1028f-108">Smart Lockout is enabled by default for all tenants and are protecting your user accounts all hello time; there is no need tooturn it on.</span></span>

<span data-ttu-id="1028f-109">Intelligens zárolás működik, hogy nyomon követi a sikertelen bejelentkezési kísérletek, és egy bizonyos után **bejelentkezési próbálkozásra van lehetőségük**kezdődően egy **kizárás időtartama**.</span><span class="sxs-lookup"><span data-stu-id="1028f-109">Smart Lockout works by keeping track of failed sign-in attempts, and after a certain **Lockout Threshold**, starting a **Lockout Duration**.</span></span> <span data-ttu-id="1028f-110">Hello támadó hello kizárás időtartama során a bejelentkezési kísérletek utasítja el.</span><span class="sxs-lookup"><span data-stu-id="1028f-110">Any sign-in attempts from hello attacker during hello Lockout Duration are rejected.</span></span> <span data-ttu-id="1028f-111">Ha hello támadás továbbra is fennáll, hello későbbi sikertelen bejelentkezési kísérletek hello kizárás időtartama leteltével fiókzárolási hosszabb időtartamokat eredményez.</span><span class="sxs-lookup"><span data-stu-id="1028f-111">If hello attack continues, hello subsequent failed sign-in attempts after hello Lockout Duration ends result in longer Lockout Durations.</span></span>

>[!NOTE]
><span data-ttu-id="1028f-112">hello alapértelmezett bejelentkezési próbálkozásra van lehetőségük, 10 sikertelen bejelentkezési kísérletek hello alapértelmezett kizárás időtartama érték 60 másodperc.</span><span class="sxs-lookup"><span data-stu-id="1028f-112">hello default Lockout Threshold is 10 failed attempts, and hello default Lockout Duration is 60 seconds.</span></span>

<span data-ttu-id="1028f-113">Intelligens zárolás is a bejelentkezések támadók és valódi felhasználók és a legtöbb esetben hello támadók csak zárolja a különböztet.</span><span class="sxs-lookup"><span data-stu-id="1028f-113">Smart Lockout also distinguishes between sign-ins from genuine users and from attackers and only locks out hello attackers in most cases.</span></span> <span data-ttu-id="1028f-114">Ez a funkció megakadályozza, hogy a támadók ártó zárolják a valódi felhasználók.</span><span class="sxs-lookup"><span data-stu-id="1028f-114">This functionality prevents attackers from maliciously locking out genuine users.</span></span> <span data-ttu-id="1028f-115">Bejelentkezési viselkedés, a felhasználói eszközök & böngészők és egyéb jelekkel toodistinguish valódi felhasználók és a támadók között túli használjuk.</span><span class="sxs-lookup"><span data-stu-id="1028f-115">We use past sign-in behavior, users’ devices & browsers and other signals toodistinguish between genuine users and attackers.</span></span> <span data-ttu-id="1028f-116">Azt a rendszer folyamatosan javítása az algoritmusok.</span><span class="sxs-lookup"><span data-stu-id="1028f-116">We are constantly improving our algorithms.</span></span>

<span data-ttu-id="1028f-117">Áteresztő hitelesítés továbbítja a jelszó érvényesítése kérelmek alakzatot a helyszíni Active Directory (AD), mert a felhasználók AD-fiókok zárolásának tooprevent támadók szüksége.</span><span class="sxs-lookup"><span data-stu-id="1028f-117">Because Pass-through Authentication forwards password validation requests onto your on-premises Active Directory (AD), you need tooprevent attackers from locking out your users’ AD accounts.</span></span> <span data-ttu-id="1028f-118">Mivel a saját AD fiókzárolási házirendek (pontosabban [ **fiókzárolás küszöbértékénél** ](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) és [ **alaphelyzetbe fiók zárolása számláló után házirendek** ](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), tooconfigure az Azure AD bejelentkezési próbálkozásra van lehetőségük van szüksége, és kizárás időtartama értékek megfelelően kimenő hello felhőben támadások toofilter elérése előtti a helyszíni AD.</span><span class="sxs-lookup"><span data-stu-id="1028f-118">Since you have your own AD Account Lockout policies (specifically, [**Account Lockout Threshold**](https://technet.microsoft.com/library/hh994574(v=ws.11).aspx) and [**Reset Account Lockout Counter After policies**](https://technet.microsoft.com/library/hh994568(v=ws.11).aspx)), you need tooconfigure Azure AD’s Lockout Threshold and Lockout Duration values appropriately toofilter out attacks in hello cloud before they reach your on-premises AD.</span></span>

>[!NOTE]
><span data-ttu-id="1028f-119">hello intelligens zárolás funkciót szabad és _a_ alapértelmezés szerint az összes ügyfél számára.</span><span class="sxs-lookup"><span data-stu-id="1028f-119">hello Smart Lockout feature is free and is _on_ by default for all customers.</span></span> <span data-ttu-id="1028f-120">Azonban az Azure AD bejelentkezési próbálkozásra van lehetőségük és fiókzárolási Duration típusú értékek Graph API-jával módosítása licencre van szüksége a bérlő toohave legalább egy Azure AD Premium P2.</span><span class="sxs-lookup"><span data-stu-id="1028f-120">However, modifying Azure AD’s Lockout Threshold and Lockout Duration values using Graph API needs your tenant toohave at least one Azure AD Premium P2 license.</span></span> <span data-ttu-id="1028f-121">Nem kell az Azure AD Premium P2 licencek _felhasználónként_ tooget hello intelligens zárolás funkciót átmenő hitelesítéssel.</span><span class="sxs-lookup"><span data-stu-id="1028f-121">You don't need an Azure AD Premium P2 license _per user_ tooget hello Smart Lockout feature with Pass-through Authentication.</span></span>

<span data-ttu-id="1028f-122">tooensure, hogy a felhasználók a helyszíni AD-fiókok védettek, tooensure van szüksége, amely:</span><span class="sxs-lookup"><span data-stu-id="1028f-122">tooensure that your users’ on-premises AD accounts are well protected, you need tooensure that:</span></span>

1.  <span data-ttu-id="1028f-123">Az Azure AD bejelentkezési próbálkozásra van lehetőségük van _kevesebb_ mint AD a fiókzárolás küszöbértékénél.</span><span class="sxs-lookup"><span data-stu-id="1028f-123">Azure AD’s Lockout Threshold is _less_ than AD’s Account Lockout Threshold.</span></span> <span data-ttu-id="1028f-124">Azt javasoljuk, hogy hello értékeket állítsa be úgy, hogy a fiókzárolás küszöbértékénél AD meg legalább két vagy három alkalommal fordult elő az Azure AD bejelentkezési próbálkozásra van lehetőségük.</span><span class="sxs-lookup"><span data-stu-id="1028f-124">We recommend that you set hello values such that AD’s Account Lockout Threshold is at least two or three times Azure AD’s Lockout Threshold.</span></span>
2.  <span data-ttu-id="1028f-125">Az Azure AD kizárás időtartama (másodpercben jelölt) _hosszabb_ mint AD meg alaphelyzetbe fiók zárolása számláló után (összességében percben megadva).</span><span class="sxs-lookup"><span data-stu-id="1028f-125">Azure AD’s Lockout Duration (represented in seconds) is _longer_ than AD’s Reset Account Lockout Counter After (represented in minutes).</span></span>

## <a name="verify-your-ad-account-lockout-policies"></a><span data-ttu-id="1028f-126">Ellenőrizze a fiók zárolása AD házirendek</span><span class="sxs-lookup"><span data-stu-id="1028f-126">Verify your AD Account Lockout policies</span></span>

<span data-ttu-id="1028f-127">Az AD a fiókzárolás házirendek használata a következő utasításokat tooverify hello:</span><span class="sxs-lookup"><span data-stu-id="1028f-127">Use hello following instructions tooverify your AD Account Lockout policies:</span></span>

1.  <span data-ttu-id="1028f-128">Hello Csoportházirend kezelése eszköz megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="1028f-128">Open hello Group Policy Management tool.</span></span>
2.  <span data-ttu-id="1028f-129">Hello csoportházirend szerkesztése, amely alkalmazott tooall felhasználók, például hello alapértelmezett tartományi házirendé.</span><span class="sxs-lookup"><span data-stu-id="1028f-129">Edit hello group policy that is applied tooall users, for example, hello Default Domain Policy.</span></span>
3.  <span data-ttu-id="1028f-130">Keresse meg a tooComputer konfigurációja\Házirendek\A beállításai\Biztonsági beállítások\Fiókházirend\Fiókzárolási házirend.</span><span class="sxs-lookup"><span data-stu-id="1028f-130">Navigate tooComputer Configuration\Policies\Windows Settings\Security Settings\Account Policies\Account Lockout Policy.</span></span>
4.  <span data-ttu-id="1028f-131">Ellenőrizze a fiókzárolás küszöbértékénél és alaphelyzetbe fiók zárolása számláló után értékeket.</span><span class="sxs-lookup"><span data-stu-id="1028f-131">Verify your Account Lockout Threshold and Reset Account Lockout Counter After values.</span></span>

![AD fiókzárolási házirendek](./media/active-directory-aadconnect-pass-through-authentication/pta5.png)

## <a name="use-hello-graph-api-toomanage-your-tenants-smart-lockout-values"></a><span data-ttu-id="1028f-133">A bérlő intelligens zárolás értékek hello Graph API toomanage használata</span><span class="sxs-lookup"><span data-stu-id="1028f-133">Use hello Graph API toomanage your tenant’s Smart Lockout values</span></span>

>[!IMPORTANT]
><span data-ttu-id="1028f-134">Az Azure AD bejelentkezési próbálkozásra van lehetőségük és fiókzárolási Duration típusú értékek Graph API-jával módosítása az Azure AD Premium P2 szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="1028f-134">Modifying Azure AD’s Lockout Threshold and Lockout Duration values using Graph API is an Azure AD Premium P2 feature.</span></span> <span data-ttu-id="1028f-135">Azt is igényli, toobe egy a bérlő globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="1028f-135">It also needs you toobe a Global Administrator on your tenant.</span></span>

<span data-ttu-id="1028f-136">Használhat [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) tooread, állítsa be, és az Azure AD intelligens zárolás értékeinek frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1028f-136">You can use [Graph Explorer](https://developer.microsoft.com/graph/graph-explorer) tooread, set, and update Azure AD’s Smart Lockout values.</span></span> <span data-ttu-id="1028f-137">Azonban programozott módon ezeket a műveleteket is van.</span><span class="sxs-lookup"><span data-stu-id="1028f-137">But you can also do these operations programmatically.</span></span>

### <a name="read-smart-lockout-values"></a><span data-ttu-id="1028f-138">Olvassa el az intelligens zárolás értékek</span><span class="sxs-lookup"><span data-stu-id="1028f-138">Read Smart Lockout values</span></span>

<span data-ttu-id="1028f-139">Kövesse ezeket a lépéseket tooread a bérlő intelligens zárolás értékeket:</span><span class="sxs-lookup"><span data-stu-id="1028f-139">Follow these steps tooread your tenant’s Smart Lockout values:</span></span>

1. <span data-ttu-id="1028f-140">A bérlő globális rendszergazdaként jelentkezzen be arra Graph Explorer.</span><span class="sxs-lookup"><span data-stu-id="1028f-140">Sign into Graph Explorer as a Global Administrator of your tenant.</span></span> <span data-ttu-id="1028f-141">Ha a rendszer kéri, adjon hozzáférést a hello kért engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="1028f-141">If prompted, grant access for hello requested permissions.</span></span>
2. <span data-ttu-id="1028f-142">"Engedélyek módosítása" gombra, és válassza ki a hello "Directory.ReadWrite.All" engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="1028f-142">Click “Modify permissions” and select hello “Directory.ReadWrite.All” permission.</span></span>
3. <span data-ttu-id="1028f-143">Az alábbiak szerint konfigurálhatja hello Graph API-kérelem: verzió beállítása túl "Béta" típushoz túl "GET" és az URL-címe túl`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span><span class="sxs-lookup"><span data-stu-id="1028f-143">Configure hello Graph API request as follows: Set version too“BETA”, request type too“GET” and URL too`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span></span>
4. <span data-ttu-id="1028f-144">Kattintson a "Lekérdezés futtatása" toosee a bérlő intelligens zárolás értékeket.</span><span class="sxs-lookup"><span data-stu-id="1028f-144">Click "Run Query" toosee your tenant's Smart Lockout values.</span></span> <span data-ttu-id="1028f-145">Ha a bérlő értékek előtt még nem állított, lásd: üres.</span><span class="sxs-lookup"><span data-stu-id="1028f-145">If you haven't set your tenant's values before, you see an empty set.</span></span>

### <a name="set-smart-lockout-values"></a><span data-ttu-id="1028f-146">Intelligens zárolás értékeinek megadása</span><span class="sxs-lookup"><span data-stu-id="1028f-146">Set Smart Lockout values</span></span>

<span data-ttu-id="1028f-147">Kövesse ezeket a lépéseket tooset a bérlő intelligens zárolás értékeit (csak az első alkalommal hello):</span><span class="sxs-lookup"><span data-stu-id="1028f-147">Follow these steps tooset your tenant’s Smart Lockout values (for hello first time only):</span></span>

1. <span data-ttu-id="1028f-148">A bérlő globális rendszergazdaként jelentkezzen be arra Graph Explorer.</span><span class="sxs-lookup"><span data-stu-id="1028f-148">Sign into Graph Explorer as a Global Administrator of your tenant.</span></span> <span data-ttu-id="1028f-149">Ha a rendszer kéri, adjon hozzáférést a hello kért engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="1028f-149">If prompted, grant access for hello requested permissions.</span></span>
2. <span data-ttu-id="1028f-150">"Engedélyek módosítása" gombra, és válassza ki a hello "Directory.ReadWrite.All" engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="1028f-150">Click “Modify permissions” and select hello “Directory.ReadWrite.All” permission.</span></span>
3. <span data-ttu-id="1028f-151">Az alábbiak szerint konfigurálhatja hello Graph API-kérelem: verzió beállítása túl "Béta" típushoz túl "POST" és az URL-címe túl`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span><span class="sxs-lookup"><span data-stu-id="1028f-151">Configure hello Graph API request as follows: Set version too“BETA”, request type too“POST” and URL too`https://graph.microsoft.com/beta/<your-tenant-domain>/settings`.</span></span>
4. <span data-ttu-id="1028f-152">Másolással illessze be a következő JSON-kérelmi hello "Kérelemtörzset" mezőbe hello.</span><span class="sxs-lookup"><span data-stu-id="1028f-152">Copy and paste hello following JSON request into hello "Request Body" field.</span></span> <span data-ttu-id="1028f-153">Hello intelligens zárolás értékeket szükség szerint módosítsa és használja egy véletlenszerű GUID Azonosítót `templateId`.</span><span class="sxs-lookup"><span data-stu-id="1028f-153">Change hello Smart Lockout values as appropriate and use a random GUID for `templateId`.</span></span>
5. <span data-ttu-id="1028f-154">Kattintson a "Lekérdezés futtatása" tooset a bérlő intelligens zárolás értékeket.</span><span class="sxs-lookup"><span data-stu-id="1028f-154">Click "Run Query" tooset your tenant's Smart Lockout values.</span></span>

```
{
  "templateId": "5cf42378-d67d-4f36-ba46-e8b86229381d",
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "300"
    },
    {
      "name": "LockoutThreshold",
      "value": "5"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

>[!NOTE]
><span data-ttu-id="1028f-155">Ha nem használ őket, meghagyhatja hello **BannedPasswordList** és **EnableBannedPasswordCheck** szerinti üres érték ("") és "false" kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="1028f-155">If you are not using them, you can leave hello **BannedPasswordList** and **EnableBannedPasswordCheck** values as empty ("") and "false" respectively.</span></span>

<span data-ttu-id="1028f-156">Győződjön meg arról, hogy beállította-e a bérlő intelligens zárolás értékek megfelelően [ezeket a lépéseket](#read-smart-lockout-values).</span><span class="sxs-lookup"><span data-stu-id="1028f-156">Verify that you have set your tenant's Smart Lockout values correctly using [these steps](#read-smart-lockout-values).</span></span>

### <a name="update-smart-lockout-values"></a><span data-ttu-id="1028f-157">Intelligens zárolás értékek frissítése</span><span class="sxs-lookup"><span data-stu-id="1028f-157">Update Smart Lockout values</span></span>

<span data-ttu-id="1028f-158">Kövesse ezeket a lépéseket tooupdate a bérlő intelligens zárolás értékek (Ha be őket előtt):</span><span class="sxs-lookup"><span data-stu-id="1028f-158">Follow these steps tooupdate your tenant’s Smart Lockout values (if you have already set them before):</span></span>

1. <span data-ttu-id="1028f-159">A bérlő globális rendszergazdaként jelentkezzen be arra Graph Explorer.</span><span class="sxs-lookup"><span data-stu-id="1028f-159">Sign into Graph Explorer as a Global Administrator of your tenant.</span></span> <span data-ttu-id="1028f-160">Ha a rendszer kéri, adjon hozzáférést a hello kért engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="1028f-160">If prompted, grant access for hello requested permissions.</span></span>
2. <span data-ttu-id="1028f-161">"Engedélyek módosítása" gombra, és válassza ki a hello "Directory.ReadWrite.All" engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="1028f-161">Click “Modify permissions” and select hello “Directory.ReadWrite.All” permission.</span></span>
3. <span data-ttu-id="1028f-162">[Kövesse ezeket a lépéseket tooread a bérlő intelligens zárolás értékek](#read-smart-lockout-values).</span><span class="sxs-lookup"><span data-stu-id="1028f-162">[Follow these steps tooread your tenant's Smart Lockout values](#read-smart-lockout-values).</span></span> <span data-ttu-id="1028f-163">Másolás hello `id` hello cikk "megjelenített"nevű, "PasswordRuleSettings" (GUID) értékét.</span><span class="sxs-lookup"><span data-stu-id="1028f-163">Copy hello `id` value (a GUID) of hello item with "displayName" as "PasswordRuleSettings".</span></span>
4. <span data-ttu-id="1028f-164">Hello Graph API-kérelem az alábbiak szerint konfigurálhatja: állítsa be a verzió túl "Béta" kéréstípus túl "Javítás" és az URL-cím túl`https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` -használata GUID Azonosítót a 3. lépés a hello `<id>`.</span><span class="sxs-lookup"><span data-stu-id="1028f-164">Configure hello Graph API request as follows: Set version too“BETA”, request type too“PATCH” and URL too`https://graph.microsoft.com/beta/<your-tenant-domain>/settings/<id>` - use hello GUID from Step 3 for `<id>`.</span></span>
5. <span data-ttu-id="1028f-165">Másolással illessze be a következő JSON-kérelmi hello "Kérelemtörzset" mezőbe hello.</span><span class="sxs-lookup"><span data-stu-id="1028f-165">Copy and paste hello following JSON request into hello "Request Body" field.</span></span> <span data-ttu-id="1028f-166">Hello intelligens zárolás értékeket szükség szerint módosítható.</span><span class="sxs-lookup"><span data-stu-id="1028f-166">Change hello Smart Lockout values as appropriate.</span></span>
6. <span data-ttu-id="1028f-167">Kattintson a "Lekérdezés futtatása" tooupdate a bérlő intelligens zárolás értékeket.</span><span class="sxs-lookup"><span data-stu-id="1028f-167">Click "Run Query" tooupdate your tenant's Smart Lockout values.</span></span>

```
{
  "values": [
    {
      "name": "LockoutDurationInSeconds",
      "value": "30"
    },
    {
      "name": "LockoutThreshold",
      "value": "4"
    },
    {
      "name" : "BannedPasswordList",
      "value": ""
    },
    {
      "name" : "EnableBannedPasswordCheck",
      "value": "false"
    }
  ]
}
```

<span data-ttu-id="1028f-168">Győződjön meg arról, hogy a bérlő intelligens zárolás értékek megfelelően frissítette [ezeket a lépéseket](#read-smart-lockout-values).</span><span class="sxs-lookup"><span data-stu-id="1028f-168">Verify that you have updated your tenant's Smart Lockout values correctly using [these steps](#read-smart-lockout-values).</span></span>

## <a name="next-steps"></a><span data-ttu-id="1028f-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1028f-169">Next steps</span></span>
- <span data-ttu-id="1028f-170">[**UserVoice** ](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) – új funkciókérések tárolásához.</span><span class="sxs-lookup"><span data-stu-id="1028f-170">[**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect) - For filing new feature requests.</span></span>
