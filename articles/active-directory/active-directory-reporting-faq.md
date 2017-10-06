---
title: "aaaAzure Active Directory Reporting – gyakori kérdések |} Microsoft Docs"
description: "Az Azure Active Directory reporting gyakran ismételt kérdések."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 534da0b1-7858-4167-9986-7a62fbd10439
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: be65a05574ea3b5b190cd02a96b211c571ba70bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting-faq"></a><span data-ttu-id="c627d-103">Azure Active Directory jelentéskészítés – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="c627d-103">Azure Active Directory reporting FAQ</span></span>

<span data-ttu-id="c627d-104">A cikk a Microsoft Azure Active Directory reporting kapcsolatban felmerülő kérdések (GYIK) válaszok toofrequently tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c627d-104">This article includes answers toofrequently asked questions (FAQs) about Azure Active Directory reporting.</span></span>  
<span data-ttu-id="c627d-105">További részletekért lásd: [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c627d-105">For more details, see [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span> 

<span data-ttu-id="c627d-106">**K: Mi hello adatok megőrzése (naplózási és bejelentkezések) tevékenységi naplóit hello Azure-portálon?**</span><span class="sxs-lookup"><span data-stu-id="c627d-106">**Q: What is hello data retention for activity logs (Audit and Sign-ins) in hello Azure portal?**</span></span> 

<span data-ttu-id="c627d-107">**V:** nyújtunk adatok 7 nap szabad ügyfeleink és tooan Azure AD Premium 1 vagy Premium 2 licenc váltásával, too30 nap fel az adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="c627d-107">**A:** We provide 7 days of data for our free customers and by switching tooan Azure AD Premium 1 or Premium 2 license, you can access data for up too30 days.</span></span> <span data-ttu-id="c627d-108">A megőrzési további részletekért lásd: [Azure Active Directory-jelentés adatmegőrzési szabályai](active-directory-reporting-retention.md)</span><span class="sxs-lookup"><span data-stu-id="c627d-108">For more details on retention, see [Azure Active Directory report retention policies](active-directory-reporting-retention.md)</span></span>

--- 

<span data-ttu-id="c627d-109">**K: mennyi ideig tart amíg I a feladat befejezése után is látható hello tevékenységek adatai?**</span><span class="sxs-lookup"><span data-stu-id="c627d-109">**Q: How long does it take until I can see hello Activity data after I have completed my task?**</span></span>

<span data-ttu-id="c627d-110">**V:** tevékenység naplókat a 15 perc tooan óra közötti késéssel rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="c627d-110">**A:** Audit activity logs have a latency ranging from 15 mins tooan hour.</span></span> <span data-ttu-id="c627d-111">Bejelentkezési tevékenység naplók a legtöbb rekordok, és néhány rekordok too2 órában 15 perc közötti késéssel rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="c627d-111">Sign-in activity logs have a latency ranging from 15 mins for most records and up too2 hours for a few records.</span></span>

---

<span data-ttu-id="c627d-112">**K: kell toobe hello Azure portál vagy tooget adatok hello API használatával bejelentkezik egy globális rendszergazdai toosee hello tevékenységet?**</span><span class="sxs-lookup"><span data-stu-id="c627d-112">**Q: Do I need toobe a global admin toosee hello activity logs in hello Azure Portal or tooget data through hello API?**</span></span>

<span data-ttu-id="c627d-113">**V.:** Nem.</span><span class="sxs-lookup"><span data-stu-id="c627d-113">**A:** No.</span></span> <span data-ttu-id="c627d-114">Akkor lehet egy **biztonsági olvasó**, egy **biztonsági rendszergazda** vagy egy **globális rendszergazda** toosee jelentéskészítéshez szükséges adatok az Azure portálon vagy keresztül hello API elérésével.</span><span class="sxs-lookup"><span data-stu-id="c627d-114">You can either be a **Security Reader**, a **Security Admin** or a **Global Admin** toosee reporting data in Azure Portal or by accessing it through hello API.</span></span>

---

<span data-ttu-id="c627d-115">**K: kaphatok Office 365 műveletnapló adatai hello Azure-portálon keresztül?**</span><span class="sxs-lookup"><span data-stu-id="c627d-115">**Q: Can I get Office 365 activity log information through hello Azure portal?**</span></span>

<span data-ttu-id="c627d-116">**V:** annak ellenére, hogy Office 365 tevékenységet és az Azure AD tevékenység naplók megosztási hello címtárerőforrásokkal, ha azt szeretné, amely teljes rálátást számos hello Office 365 tevékenységi naplóit, toohello Office 365 felügyeleti központban tooget Office 365 műveletnapló adatai kell lépjen.</span><span class="sxs-lookup"><span data-stu-id="c627d-116">**A:** Even though Office 365 activity and Azure AD activity logs share a lot of hello directory resources, if you want a full view of hello Office 365 activity logs, you should go toohello Office 365 Admin Center tooget Office 365 Activity log information.</span></span>

---


<span data-ttu-id="c627d-117">**K: amelyek API-k használata Office 365 tevékenységi naplóit tooget információ?**</span><span class="sxs-lookup"><span data-stu-id="c627d-117">**Q: Which APIs do I use tooget information about Office 365 Activity logs?**</span></span>

<span data-ttu-id="c627d-118">**V:** használata hello Office 365 felügyeleti API-k tooaccess hello [Office 365 tevékenység naplózza az API-n keresztül](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).</span><span class="sxs-lookup"><span data-stu-id="c627d-118">**A:** Use hello Office 365 Management APIs tooaccess hello [Office 365 Activity logs through an API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).</span></span>

---

<span data-ttu-id="c627d-119">**K: hogyan sok rekord letölthető Azure-portálon?**</span><span class="sxs-lookup"><span data-stu-id="c627d-119">**Q: How many records I can download from Azure portal?**</span></span>

<span data-ttu-id="c627d-120">**V:** töltheti fel too120K bejegyzéseit hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="c627d-120">**A:** You can download up too120K records from hello Azure portal.</span></span> <span data-ttu-id="c627d-121">hello rekordok alapján vannak rendezve *legutóbbi* és alapértelmezés szerint le hello legutóbbi 120-K rögzíti.</span><span class="sxs-lookup"><span data-stu-id="c627d-121">hello records are sorted by *most recent* and by default, you get hello most recent 120K records.</span></span> 

---

<span data-ttu-id="c627d-122">**K: hogyan sok rekord tudja lekérdezni hello tevékenységek API keresztül?**</span><span class="sxs-lookup"><span data-stu-id="c627d-122">**Q: How many records can I query through hello activities API?**</span></span>

<span data-ttu-id="c627d-123">**V:** too1 millió rekordok kérdezhetők le (Ha nem adja meg hello top operátor, többsége hello rekord rendező legutóbbi).</span><span class="sxs-lookup"><span data-stu-id="c627d-123">**A:** You can query up too1 million records (if you don’t use hello top operator, which sorts hello record by most recent).</span></span> <span data-ttu-id="c627d-124">Ha hello "top" operátort használja, lekérheti too500K rekordok.</span><span class="sxs-lookup"><span data-stu-id="c627d-124">If you do use hello “top” operator, you can query up too500K records.</span></span> <span data-ttu-id="c627d-125">Mintalekérdezések a módját a toouse hello API itt található [Itt](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="c627d-125">You can find sample queries on how toouse hello API here [here](active-directory-reporting-api-getting-started.md).</span></span>

---

<span data-ttu-id="c627d-126">**K: Hogyan tudom működtetni premium licenc?**</span><span class="sxs-lookup"><span data-stu-id="c627d-126">**Q: How do I get a premium license?**</span></span>

<span data-ttu-id="c627d-127">**V:** lásd [Ismerkedés az Azure Active Directory Premium](active-directory-get-started-premium.md) egy válasz toothis kérdés.</span><span class="sxs-lookup"><span data-stu-id="c627d-127">**A:** See [Getting started with Azure Active Directory Premium](active-directory-get-started-premium.md) for an answer toothis question.</span></span>

---

<span data-ttu-id="c627d-128">**K: hogyan hamarosan kell meg a tevékenységek adatok premium licenc beolvasása után?**</span><span class="sxs-lookup"><span data-stu-id="c627d-128">**Q: How soon should I see activities data after getting a premium license?**</span></span>

<span data-ttu-id="c627d-129">**V:** Ha szabad licenc tevékenységek az adatok már rendelkezik, akkor láthatja hello ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="c627d-129">**A:** If you already have activities data as a free license, then you can see hello same data.</span></span> <span data-ttu-id="c627d-130">Ha még nem rendelkezik az adatokat, majd tart egy vagy két napot.</span><span class="sxs-lookup"><span data-stu-id="c627d-130">If you don’t have any data, then it will take one or two days.</span></span>

---

<span data-ttu-id="c627d-131">**K: jelennek meg adatok előző hónap után egy Azure AD premium-licenc beolvasásakor?**</span><span class="sxs-lookup"><span data-stu-id="c627d-131">**Q: Do I see last month's data after getting an Azure AD premium license?**</span></span>

<span data-ttu-id="c627d-132">**V:** nemrég váltottunk tooa prémium verzió (beleértve a rendszer próbaverzióját), ha látható adatok mentése too7 nap kezdetben.</span><span class="sxs-lookup"><span data-stu-id="c627d-132">**A:** If you have recently switched tooa Premium version (including a trial version), you can see data up too7 days initially.</span></span> <span data-ttu-id="c627d-133">Összesít adatokat, amikor látni fogja a másolatot too30 nap.</span><span class="sxs-lookup"><span data-stu-id="c627d-133">When data accumulates, you will see up too30 days.</span></span>

---

<span data-ttu-id="c627d-134">**K: van a kockázati események azonosító adatok védelmét, de nem látható az megfelelő bejelentkezés hello az összes bejelentkezéseket. Ez várható?**</span><span class="sxs-lookup"><span data-stu-id="c627d-134">**Q: There is a risk event in Identity Protection but I’m not seeing corresponding sign-in in hello all sign-ins. Is this expected?**</span></span>

<span data-ttu-id="c627d-135">**V:** Igen, Identity Protection kiértékeli minden hitelesítési forgalom kockázatát e ha interaktív vagy nem interaktív.</span><span class="sxs-lookup"><span data-stu-id="c627d-135">**A:** Yes, Identity Protection evaluates risk for all authentication flows whether if be interactive or non-interactive.</span></span> <span data-ttu-id="c627d-136">Azonban minden bejelentkezések csak jelentésben csak hello interaktív bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="c627d-136">However, all sign-ins only report shows only hello interactive sign-ins.</span></span>

---

<span data-ttu-id="c627d-137">**K: hogyan töltheti hello Azure-portálon "Kockázat megjelölt felhasználók" jelentést?**</span><span class="sxs-lookup"><span data-stu-id="c627d-137">**Q: How can I download hello “Users flagged for risk” report in Azure portal?**</span></span>

<span data-ttu-id="c627d-138">**V:** hello beállítás toodownload *felhasználók meg van jelölve, a kockázat* jelentés hamarosan megjelenik.</span><span class="sxs-lookup"><span data-stu-id="c627d-138">**A:** hello option toodownload *Users flagged for risk* report will be added soon.</span></span>

---

<span data-ttu-id="c627d-139">**K: Hogyan állapítható meg, hogy miért a bejelentkezés vagy a felhasználó lett megjelölve hello Azure-portálon a kockázatos?**</span><span class="sxs-lookup"><span data-stu-id="c627d-139">**Q: How do I know why a sign-in or a user was flagged risky in hello Azure portal?**</span></span>

<span data-ttu-id="c627d-140">**V:** Premium edition ügyfelek talál további információt az alapul szolgáló kockázati események ehhez kattintson a "Felhasználók megjelölt kockázatot jelent a" hello felhasználó vagy a "Kockázatos sign-ins" hello kattintva hello.</span><span class="sxs-lookup"><span data-stu-id="c627d-140">**A:** Premium edition customers can learn more about hello underlying risk events by clicking on hello user in “Users flagged for risk” or by clicking on hello “Risky sign-ins”.</span></span> <span data-ttu-id="c627d-141">Szabad és alapvető edition ügyfelek le toosee hello at-risk felhasználók és a bejelentkezések nélkül hello az alapul szolgáló kockázati események adatait.</span><span class="sxs-lookup"><span data-stu-id="c627d-141">Free and Basic edition customers get toosee hello at-risk users and sign-ins without hello underlying risk event information.</span></span>

---

<span data-ttu-id="c627d-142">**K: hogyan kiszámítása hello bejelentkezéseket és kockázatos bejelentkezések jelentés IP-címek??**</span><span class="sxs-lookup"><span data-stu-id="c627d-142">**Q: How are IP addresses calculated in hello sign-ins and risky sign-ins report??**</span></span>

<span data-ttu-id="c627d-143">**V:** IP-címek kiállított úgy, hogy nincs-e végleges kapcsolat közötti IP-cím és hello számítógép ezzel a címmel rendelkező fizikai helyét.</span><span class="sxs-lookup"><span data-stu-id="c627d-143">**A:** IP addresses are issued in such a way that there is no definitive connection between an IP address and where hello computer with that address is physically located.</span></span> <span data-ttu-id="c627d-144">Ez például a mobil szolgáltatók és a kiállító központi készletek IP-címek nagyon gyakran messze hol használják ténylegesen hello ügyféleszköz VPN tényezőktől bonyolítja.</span><span class="sxs-lookup"><span data-stu-id="c627d-144">This is complicated by factors such as mobile providers and VPNs issuing IP addresses from central pools often very far from where hello client device is actually used.</span></span> <span data-ttu-id="c627d-145">Megadott fenti hello, IP-cím tooa fizikai hely konvertálása a nyomkövetési adatokat, beállításjegyzék-adatok, fordított keresések és egyéb információk alapján a lehető legkedvezőbb módon.</span><span class="sxs-lookup"><span data-stu-id="c627d-145">Given hello above, converting IP address tooa physical location is a best effort based on traces, registry data, reverse look ups and other information.</span></span> 

---
