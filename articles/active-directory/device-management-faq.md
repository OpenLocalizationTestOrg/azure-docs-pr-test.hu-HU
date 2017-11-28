---
title: "aaaAzure Active Directory eszköz felügyeleti kapcsolatos gyakori kérdések |} Microsoft Docs"
description: "Az Azure Active Directory kezelése – gyakori kérdések."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 000eb6a930187e13cb24cf628793afd06813be23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-device-management-faq"></a><span data-ttu-id="ad26c-103">Azure Active Directory eszköz felügyeleti kapcsolatos gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="ad26c-103">Azure Active Directory device management FAQ</span></span>

<span data-ttu-id="ad26c-104">**K: I hello eszköz nemrég regisztrálva. Miért nem látom hello eszköz hello Azure-portálon a saját felhasználói adatok alapján?**</span><span class="sxs-lookup"><span data-stu-id="ad26c-104">**Q: I registered hello device recently. Why can’t I see hello device under my user info in hello Azure portal?**</span></span>

<span data-ttu-id="ad26c-105">**V:** , amelyek automatikus eszközregisztráció tartományhoz csatlakoztatott Windows 10-eszközök nem jelennek meg hello felhasználói adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="ad26c-105">**A:** Windows 10 devices that are domain-joined with automatic device registration do not show up under hello USER info.</span></span>
<span data-ttu-id="ad26c-106">Minden eszköz kell toouse PowerShell toosee.</span><span class="sxs-lookup"><span data-stu-id="ad26c-106">You need toouse PowerShell toosee all devices.</span></span> 

<span data-ttu-id="ad26c-107">Csak hello következő eszköz szerepel-e hello felhasználói adatok alapján:</span><span class="sxs-lookup"><span data-stu-id="ad26c-107">Only hello following devices are listed under hello USER info:</span></span>

- <span data-ttu-id="ad26c-108">Az összes személyes eszközök, amelyek nem csatlakoztatott vállalati</span><span class="sxs-lookup"><span data-stu-id="ad26c-108">All personal devices that are not enterprise joined</span></span> 
- <span data-ttu-id="ad26c-109">Az összes nem - Windows 10 vagy Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="ad26c-109">All non-Windows 10 / Windows Server 2016</span></span> 
- <span data-ttu-id="ad26c-110">Minden-Windows eszköz</span><span class="sxs-lookup"><span data-stu-id="ad26c-110">All non-Windows devices</span></span> 

---

<span data-ttu-id="ad26c-111">**K: Miért nem látom minden hello eszköz hello Azure-portálon az Azure Active Directoryban regisztrálva?**</span><span class="sxs-lookup"><span data-stu-id="ad26c-111">**Q: Why can I not see all hello devices registered in Azure Active Directory in hello Azure portal?**</span></span> 

<span data-ttu-id="ad26c-112">**V:** jelenleg nincs módja toosee minden regisztrált eszközöket a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="ad26c-112">**A:** Currently, there is no way toosee all registered devices in hello Azure portal.</span></span> <span data-ttu-id="ad26c-113">Azure PowerShell toofind minden eszköz használható.</span><span class="sxs-lookup"><span data-stu-id="ad26c-113">You can use Azure PowerShell toofind all devices.</span></span> <span data-ttu-id="ad26c-114">További részletekért lásd: hello [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="ad26c-114">For more details, see hello [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet.</span></span>

--- 

<span data-ttu-id="ad26c-115">**K: Hogyan állapítható meg, milyen hello eszköz regisztrációs állapotát hello ügyfél van?**</span><span class="sxs-lookup"><span data-stu-id="ad26c-115">**Q: How do I know what hello device registration state of hello client is?**</span></span>

<span data-ttu-id="ad26c-116">**V:** hello eszköz regisztrációs állapotától függ:</span><span class="sxs-lookup"><span data-stu-id="ad26c-116">**A:** hello device registration state depends on:</span></span>

- <span data-ttu-id="ad26c-117">Milyen hello eszköz</span><span class="sxs-lookup"><span data-stu-id="ad26c-117">What hello device is</span></span>
- <span data-ttu-id="ad26c-118">Hogyan regisztrálták</span><span class="sxs-lookup"><span data-stu-id="ad26c-118">How it was registered</span></span> 
- <span data-ttu-id="ad26c-119">Minden adatát tooit kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="ad26c-119">Any details related tooit.</span></span> 
 

---

<span data-ttu-id="ad26c-120">**K: Miért történik szeretnék, hogy törölték a hello Azure portál vagy a Windows PowerShell használatával továbbra is szerepel a regisztrált?**</span><span class="sxs-lookup"><span data-stu-id="ad26c-120">**Q: Why is a device I have deleted in hello Azure portal or using Windows PowerShell still listed as registered?**</span></span>

<span data-ttu-id="ad26c-121">**V:** Ez az elvárt működés.</span><span class="sxs-lookup"><span data-stu-id="ad26c-121">**A:** This is by design.</span></span> <span data-ttu-id="ad26c-122">hello eszköz nem fog hozzáférést tooresources hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="ad26c-122">hello device will not have access tooresources in hello cloud.</span></span> <span data-ttu-id="ad26c-123">Ha szeretné, hogy tooremove hello eszközt, és regisztrálja újra, a manuális műveletet végzett hello eszköz toobe kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ad26c-123">If you want tooremove hello device and register again, a manual action must be toobe taken on hello device.</span></span> 

<span data-ttu-id="ad26c-124">A Windows 10 és Windows Server 2016-os, amely a helyszíni AD-tartományhoz:</span><span class="sxs-lookup"><span data-stu-id="ad26c-124">For Windows 10 and Windows Server 2016 that are on-premises AD domain-joined:</span></span>

1.  <span data-ttu-id="ad26c-125">Nyissa meg a hello parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="ad26c-125">Open hello command prompt as an administrator.</span></span>

2.  <span data-ttu-id="ad26c-126">Típusa`dsregcmd.exe /debug /leave`</span><span class="sxs-lookup"><span data-stu-id="ad26c-126">Type `dsregcmd.exe /debug /leave`</span></span>

3.  <span data-ttu-id="ad26c-127">Jelentkezzen ki, és jelentkezzen be tootrigger hello ütemezett feladatot, amely regisztrálja újra a hello eszközt.</span><span class="sxs-lookup"><span data-stu-id="ad26c-127">Sign out and sign in tootrigger hello scheduled task that registers hello device again.</span></span> 

<span data-ttu-id="ad26c-128">Az egyéb Windows platformra, amelyek a helyszíni AD-tartományhoz:</span><span class="sxs-lookup"><span data-stu-id="ad26c-128">For other Windows platforms that are on-premises AD domain-joined:</span></span>

1.  <span data-ttu-id="ad26c-129">Nyissa meg a hello parancssort rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="ad26c-129">Open hello command prompt as an administrator.</span></span>
2.  <span data-ttu-id="ad26c-130">Gépelje be: `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.</span><span class="sxs-lookup"><span data-stu-id="ad26c-130">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.</span></span>
3.  <span data-ttu-id="ad26c-131">Gépelje be: `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.</span><span class="sxs-lookup"><span data-stu-id="ad26c-131">Type `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.</span></span>

---

<span data-ttu-id="ad26c-132">**Kérdés: Miért látom azt ismétlődő eszközbevitelek Azure-portálon?**</span><span class="sxs-lookup"><span data-stu-id="ad26c-132">**Q: Why do I see duplicate device entries in Azure portal?**</span></span>

<span data-ttu-id="ad26c-133">**V:**</span><span class="sxs-lookup"><span data-stu-id="ad26c-133">**A:**</span></span>

-   <span data-ttu-id="ad26c-134">A Windows 10 és Windows Server 2016, ha az ismételt kísérletek toounjoin és újracsatlakozhatnak hello ugyanarra az eszközre, előfordulhat ismétlődő bejegyzések.</span><span class="sxs-lookup"><span data-stu-id="ad26c-134">For Windows 10 and Windows Server 2016, if they are repeated attempts toounjoin and re-join hello same device, there might be duplicate entries.</span></span> 

-   <span data-ttu-id="ad26c-135">Ha használta, adja hozzá a munkahelyi vagy iskolai fiókkal, minden windows-felhasználó hozzáadása munkahelyi vagy iskolai fiókhoz használó fog hozzon létre egy új eszköz rekordot hello ugyanazon eszköznév.</span><span class="sxs-lookup"><span data-stu-id="ad26c-135">If you have used Add Work or School Account, each windows user who uses Add Work or School Account will create a new device record with hello same device name.</span></span>

-   <span data-ttu-id="ad26c-136">Egyéb Windows-platformokat, amelyek a helyszíni AD tartományhoz az automatikus regisztráció használatával hoz létre egy új eszközrekordhoz hello minden tartományi felhasználó bejelentkezik az hello eszköz azonos eszköznevet.</span><span class="sxs-lookup"><span data-stu-id="ad26c-136">Other Windows platforms that are on-premises AD domain-joined using automatic registration will create a new device record with hello same device name for each domain user who logs into hello device.</span></span> 

-   <span data-ttu-id="ad26c-137">Az, hogy törölték, AADJ gép újratelepítése, és újra csatlakoztatni a hello azonos nevet, egy másik rekord, hello állapotúként jelenik meg ugyanazon eszköznév.</span><span class="sxs-lookup"><span data-stu-id="ad26c-137">An AADJ machine that has been wiped, re-installed and re-joined with hello same name, will show up as another record with hello same device name.</span></span>

---

<span data-ttu-id="ad26c-138">**K: Miért egy felhasználó erőforrásaihoz is hozzáférjenek az eszközről szeretnék letiltotta a hello Azure-portálon?**</span><span class="sxs-lookup"><span data-stu-id="ad26c-138">**Q: Why can a user still access resources from a device I have disabled in hello Azure portal?**</span></span>

<span data-ttu-id="ad26c-139">**V:** be egy alkalmazott a visszavonási toobe tooan órát is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="ad26c-139">**A:** It can take up tooan hour for a revoke toobe applied.</span></span>

>[!Note] 
><span data-ttu-id="ad26c-140">Az elveszett eszközök ajánlott hello eszköz tooensure, hogy a felhasználók nem férhetnek hozzá a hello eszköz törlése.</span><span class="sxs-lookup"><span data-stu-id="ad26c-140">For lost devices, we recommend wiping hello device tooensure that users cannot access hello device.</span></span> <span data-ttu-id="ad26c-141">További részletekért lásd: [eszközök regisztrálása az Intune-ban felügyeletre](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).</span><span class="sxs-lookup"><span data-stu-id="ad26c-141">For more details, see [Enroll devices for management in Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune).</span></span> 


---

<span data-ttu-id="ad26c-142">**K: Miért látnak a felhasználók "Nem érheti el innen"?**</span><span class="sxs-lookup"><span data-stu-id="ad26c-142">**Q: Why do my users see “You can’t get there from here”?**</span></span>

<span data-ttu-id="ad26c-143">**V:** konfigurált egyes feltételes hozzáférési szabályok toorequire egy adott eszköz állapotát, és hello eszköz nem felel meg a hello feltételek, ha a felhasználók le vannak tiltva, és ez az üzenet megjelenik.</span><span class="sxs-lookup"><span data-stu-id="ad26c-143">**A:** If you have configured certain conditional access rules toorequire a specific device state and hello device does not meet hello criteria, users are blocked and see this message.</span></span> <span data-ttu-id="ad26c-144">Értékelje ki a hello szabályok, és győződjön meg arról, hogy hello eszköz képes toomeet hello feltételek tooavoid van ezt az üzenetet.</span><span class="sxs-lookup"><span data-stu-id="ad26c-144">Please evaluate hello rules and ensure that hello device is able toomeet hello criteria tooavoid this message.</span></span>

---


<span data-ttu-id="ad26c-145">**K: I lásd: hello rekordokat az Azure-portálon hello hello felhasználói adatok alapján, és hello ügyfélen regisztrált hello állapota látható. A feltételes hozzáférés használatának megfelelően beállítani a perceké 'M?**</span><span class="sxs-lookup"><span data-stu-id="ad26c-145">**Q: I see hello device record under hello USER info in hello Azure portal and can see hello state as registered on hello client. Am I setup correctly for using conditional access?**</span></span>

<span data-ttu-id="ad26c-146">**V:** hello eszközrekordhoz (deviceID) és az állapotot a hello Azure-portálon kell hello ügyfél felel meg, és bármely értékelési feltételeknek a feltételes hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="ad26c-146">**A:** hello device record (deviceID) and state on hello Azure portal must match hello client and meet any evaluation criteria for conditional access.</span></span> <span data-ttu-id="ad26c-147">További részletekért lásd: [Ismerkedés az Azure Active Directory Eszközregisztrációs](active-directory-device-registration.md).</span><span class="sxs-lookup"><span data-stu-id="ad26c-147">For more details, see [Get started with Azure Active Directory Device Registration](active-directory-device-registration.md).</span></span>

---

<span data-ttu-id="ad26c-148">**K: Miért kapok "felhasználónév vagy jelszó nem megfelelő" üzenet egy eszközhöz csak csatlakozott, I tooAzure AD?**</span><span class="sxs-lookup"><span data-stu-id="ad26c-148">**Q: Why do I get a "username or password is incorrect" message for a device I have just joined tooAzure AD?**</span></span>

<span data-ttu-id="ad26c-149">**V:** Ez a forgatókönyv gyakori okai a következők:</span><span class="sxs-lookup"><span data-stu-id="ad26c-149">**A:** Common reasons for this scenario are:</span></span>

- <span data-ttu-id="ad26c-150">A felhasználói hitelesítő adatok nem érvényesek.</span><span class="sxs-lookup"><span data-stu-id="ad26c-150">Your user credentials are no longer valid.</span></span>

- <span data-ttu-id="ad26c-151">A számítógép nem toocommunicate az Azure Active Directoryval.</span><span class="sxs-lookup"><span data-stu-id="ad26c-151">Your computer is unable toocommunicate with Azure Active Directory.</span></span> <span data-ttu-id="ad26c-152">Ellenőrizze, hogy minden hálózati kapcsolati problémáit.</span><span class="sxs-lookup"><span data-stu-id="ad26c-152">Check for any network connectivity issues.</span></span>

- <span data-ttu-id="ad26c-153">hello Azure AD Join előfeltételek nem teljesültek.</span><span class="sxs-lookup"><span data-stu-id="ad26c-153">hello Azure AD Join pre-requisites were not met.</span></span> <span data-ttu-id="ad26c-154">Győződjön meg arról, hogy elvégezte-e hello lépései [kiterjesztése a felhőalapú képességek tooWindows 10 eszközöknek az Azure Active Directory csatlakozási](active-directory-azureadjoin-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ad26c-154">Please ensure that you have followed hello steps for [Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join](active-directory-azureadjoin-overview.md).</span></span>  

- <span data-ttu-id="ad26c-155">Összevont bejelentkezések igényel az összevonási kiszolgáló toosupport egy WS-Trust aktív végpontot.</span><span class="sxs-lookup"><span data-stu-id="ad26c-155">Federated logins requires your federation server toosupport a WS-Trust active endpoint.</span></span> 

---

<span data-ttu-id="ad26c-156">**K: Miért látom azt hello "Oops... hiba!" párbeszédpanel jelenik meg a számítógép csatlakozni?**</span><span class="sxs-lookup"><span data-stu-id="ad26c-156">**Q: Why do I see hello “Oops… an error occurred!" dialog when I try do join my PC?**</span></span>

<span data-ttu-id="ad26c-157">**V:** Ez az Azure Active Directory-regisztráció az Intune-nal beállításának eredménye.</span><span class="sxs-lookup"><span data-stu-id="ad26c-157">**A:** This is a result of setting up Azure Active Directory enrollment with Intune.</span></span> <span data-ttu-id="ad26c-158">További részletekért lásd: [Windows-eszközök kezelésének beállítása](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).</span><span class="sxs-lookup"><span data-stu-id="ad26c-158">For more details, see [Set up Windows device management](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).</span></span>  

---

<span data-ttu-id="ad26c-159">**K: Miért a kísérlet toojoin egy PC sikertelen lesz, bár kaptam bármely hibainformációk?**</span><span class="sxs-lookup"><span data-stu-id="ad26c-159">**Q: Why did my attempt toojoin a PC fail although I didn't get any error information?**</span></span>

<span data-ttu-id="ad26c-160">**V:** ennek valószínű oka az, hogy hello felhasználó van bejelentkezve toohello eszköz hello beépített rendszergazdai fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="ad26c-160">**A:** A likely cause is that hello user is logged in toohello device using hello built-in administrator account.</span></span> <span data-ttu-id="ad26c-161">Hozzon létre egy másik helyi fiók használata az Azure Active Directory csatlakozási toocomplete hello beállítása előtt.</span><span class="sxs-lookup"><span data-stu-id="ad26c-161">Please create a different local account before using Azure Active Directory Join toocomplete hello setup.</span></span> 

---

<span data-ttu-id="ad26c-162">**K: hol található hello beállítása automatikus eszközregisztráció utasításait?**</span><span class="sxs-lookup"><span data-stu-id="ad26c-162">**Q: Where can I find instructions for hello setup of automatic device registration?**</span></span>

<span data-ttu-id="ad26c-163">**V:** részletes útmutatásért lásd: [hogyan tooconfigure az automatikus regisztráció, a Windows-tartományhoz az Azure Active Directoryval eszközök](active-directory-conditional-access-automatic-device-registration-setup.md)</span><span class="sxs-lookup"><span data-stu-id="ad26c-163">**A:** For detailed instructions, see [How tooconfigure automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md)</span></span>

---

<span data-ttu-id="ad26c-164">**K: hol található a hibaelhárítási információkat hello automatikus eszközregisztráció?**</span><span class="sxs-lookup"><span data-stu-id="ad26c-164">**Q: Where can I find troubleshooting information about hello automatic device registration?**</span></span>

<span data-ttu-id="ad26c-165">**V:** a hibaelhárítással kapcsolatos információkért, lásd:</span><span class="sxs-lookup"><span data-stu-id="ad26c-165">**A:** For troubleshooting information, see:</span></span>

- [<span data-ttu-id="ad26c-166">Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépek tooAzure AD – Windows 10 és Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="ad26c-166">Troubleshooting auto-registration of domain joined computers tooAzure AD – Windows 10 and Windows Server 2016</span></span>](active-directory-device-registration-troubleshoot-windows.md)

- [<span data-ttu-id="ad26c-167">Hibaelhárítás az automatikus regisztráció tartomány csatlakoztatott számítógépek tooAzure AD Windows régebbi ügyfelek</span><span class="sxs-lookup"><span data-stu-id="ad26c-167">Troubleshooting auto-registration of domain joined computers tooAzure AD for Windows down-level clients</span></span>](active-directory-device-registration-troubleshoot-windows-legacy.md)
 
---

