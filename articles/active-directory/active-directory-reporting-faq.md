---
title: "Azure Active Directory jelentéskészítés – gyakori kérdések |} Microsoft Docs"
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
ms.openlocfilehash: accf292f70bf0eafdefc00c3feeaf8e346605401
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-reporting-faq"></a><span data-ttu-id="ff2e2-103">Azure Active Directory jelentéskészítés – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="ff2e2-103">Azure Active Directory reporting FAQ</span></span>

<span data-ttu-id="ff2e2-104">Ez a cikk kapcsolatos gyakori kérdések (GYIK) Azure Active Directory reporting rájuk adott válaszokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-104">This article includes answers to frequently asked questions (FAQs) about Azure Active Directory reporting.</span></span>  
<span data-ttu-id="ff2e2-105">További részletekért lásd: [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ff2e2-105">For more details, see [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span> 

<span data-ttu-id="ff2e2-106">**K: Mi az az adatok megőrzése (naplózási és bejelentkezések) tevékenységi naplóit Azure-portálon?**</span><span class="sxs-lookup"><span data-stu-id="ff2e2-106">**Q: What is the data retention for activity logs (Audit and Sign-ins) in the Azure portal?**</span></span> 

<span data-ttu-id="ff2e2-107">**V:** nyújtunk adatok 7 nap szabad ügyfelei és az Azure AD Premium 1 vagy Premium 2 licenc váltásával, akár 30 napig lehet elérni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-107">**A:** We provide 7 days of data for our free customers and by switching to an Azure AD Premium 1 or Premium 2 license, you can access data for up to 30 days.</span></span> <span data-ttu-id="ff2e2-108">A megőrzési további részletekért lásd: [Azure Active Directory-jelentés adatmegőrzési szabályai](active-directory-reporting-retention.md)</span><span class="sxs-lookup"><span data-stu-id="ff2e2-108">For more details on retention, see [Azure Active Directory report retention policies](active-directory-reporting-retention.md)</span></span>

--- 

<span data-ttu-id="ff2e2-109">**K: mennyi ideig tart amíg I a feladat befejezése után is látható a tevékenységek adatai?**</span><span class="sxs-lookup"><span data-stu-id="ff2e2-109">**Q: How long does it take until I can see the Activity data after I have completed my task?**</span></span>

<span data-ttu-id="ff2e2-110">**V:** tevékenység naplókat a 15 perc és egy óra közötti késéssel rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-110">**A:** Audit activity logs have a latency ranging from 15 mins to an hour.</span></span> <span data-ttu-id="ff2e2-111">Bejelentkezési tevékenység naplók rendelkezik a legtöbb rekordok 15 perc közötti késleltetés és néhány rekord két óra.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-111">Sign-in activity logs have a latency ranging from 15 mins for most records and up to 2 hours for a few records.</span></span>

---

<span data-ttu-id="ff2e2-112">**K: kell lennie a globális rendszergazdának kell tekintse meg a tevékenység naplózza az Azure portálon vagy az adatok lekérése az API-n keresztül?**</span><span class="sxs-lookup"><span data-stu-id="ff2e2-112">**Q: Do I need to be a global admin to see the activity logs in the Azure Portal or to get data through the API?**</span></span>

<span data-ttu-id="ff2e2-113">**V.:** Nem.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-113">**A:** No.</span></span> <span data-ttu-id="ff2e2-114">Akkor lehet egy **biztonsági olvasó**, egy **biztonsági rendszergazda** vagy egy **globális rendszergazda** megtekintéséhez a jelentéskészítéshez szükséges adatok az Azure portálon vagy az API-n keresztül elérésével.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-114">You can either be a **Security Reader**, a **Security Admin** or a **Global Admin** to see reporting data in Azure Portal or by accessing it through the API.</span></span>

---

<span data-ttu-id="ff2e2-115">**K: kaphatok Office 365 műveletnapló adatai az Azure portálon keresztül?**</span><span class="sxs-lookup"><span data-stu-id="ff2e2-115">**Q: Can I get Office 365 activity log information through the Azure portal?**</span></span>

<span data-ttu-id="ff2e2-116">**V:** annak ellenére, hogy Office 365 tevékenységet, és az Azure AD tevékenység naplók megosztás a könyvtár-erőforrások számos, ha azt szeretné, hogy az Office 365 tevékenységi naplóit, teljes egészében meg kell lépjen az Office 365 felügyeleti központban Office 365 tevékenység naplózási adatok beszerzése.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-116">**A:** Even though Office 365 activity and Azure AD activity logs share a lot of the directory resources, if you want a full view of the Office 365 activity logs, you should go to the Office 365 Admin Center to get Office 365 Activity log information.</span></span>

---


<span data-ttu-id="ff2e2-117">**K: milyen API-k használata Office 365 tevékenységi naplóit kapcsolatos adatok**</span><span class="sxs-lookup"><span data-stu-id="ff2e2-117">**Q: Which APIs do I use to get information about Office 365 Activity logs?**</span></span>

<span data-ttu-id="ff2e2-118">**V:** eléréséhez használja az Office 365 felügyeleti API-kat a [Office 365 tevékenység naplózza az API-n keresztül](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).</span><span class="sxs-lookup"><span data-stu-id="ff2e2-118">**A:** Use the Office 365 Management APIs to access the [Office 365 Activity logs through an API](https://msdn.microsoft.com/office-365/office-365-managment-apis-overview).</span></span>

---

<span data-ttu-id="ff2e2-119">**K: hogyan sok rekord letölthető Azure-portálon?**</span><span class="sxs-lookup"><span data-stu-id="ff2e2-119">**Q: How many records I can download from Azure portal?**</span></span>

<span data-ttu-id="ff2e2-120">**V:** legfeljebb 120 K rekordok tölthet le az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-120">**A:** You can download up to 120K records from the Azure portal.</span></span> <span data-ttu-id="ff2e2-121">A rekordok alapján vannak rendezve *legutóbbi* és alapértelmezés szerint le a legfrissebb 120 K rögzíti.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-121">The records are sorted by *most recent* and by default, you get the most recent 120K records.</span></span> 

---

<span data-ttu-id="ff2e2-122">**K: hogyan sok rekord tudja lekérdezni a tevékenységek API keresztül?**</span><span class="sxs-lookup"><span data-stu-id="ff2e2-122">**Q: How many records can I query through the activities API?**</span></span>

<span data-ttu-id="ff2e2-123">**V:** legfeljebb 1 millió rekordot lekérheti (Ha nem adja meg a top operátor, többsége a rekord rendező legutóbbi).</span><span class="sxs-lookup"><span data-stu-id="ff2e2-123">**A:** You can query up to 1 million records (if you don’t use the top operator, which sorts the record by most recent).</span></span> <span data-ttu-id="ff2e2-124">Ha a "top" operátort használja, lekérheti mentése 500 KB-os rekordok.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-124">If you do use the “top” operator, you can query up to 500K records.</span></span> <span data-ttu-id="ff2e2-125">Mintalekérdezések a API használatáról itt található [Itt](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="ff2e2-125">You can find sample queries on how to use the API here [here](active-directory-reporting-api-getting-started.md).</span></span>

---

<span data-ttu-id="ff2e2-126">**K: Hogyan tudom működtetni premium licenc?**</span><span class="sxs-lookup"><span data-stu-id="ff2e2-126">**Q: How do I get a premium license?**</span></span>

<span data-ttu-id="ff2e2-127">**V:** lásd [Ismerkedés az Azure Active Directory Premium](active-directory-get-started-premium.md) a választ a kérdésére.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-127">**A:** See [Getting started with Azure Active Directory Premium](active-directory-get-started-premium.md) for an answer to this question.</span></span>

---

<span data-ttu-id="ff2e2-128">**K: hogyan hamarosan kell meg a tevékenységek adatok premium licenc beolvasása után?**</span><span class="sxs-lookup"><span data-stu-id="ff2e2-128">**Q: How soon should I see activities data after getting a premium license?**</span></span>

<span data-ttu-id="ff2e2-129">**V:** Ha szabad licenc tevékenységek az adatok már rendelkezik, akkor ugyanazokat az adatokat láthatja.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-129">**A:** If you already have activities data as a free license, then you can see the same data.</span></span> <span data-ttu-id="ff2e2-130">Ha még nem rendelkezik az adatokat, majd tart egy vagy két napot.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-130">If you don’t have any data, then it will take one or two days.</span></span>

---

<span data-ttu-id="ff2e2-131">**K: jelennek meg adatok előző hónap után egy Azure AD premium-licenc beolvasásakor?**</span><span class="sxs-lookup"><span data-stu-id="ff2e2-131">**Q: Do I see last month's data after getting an Azure AD premium license?**</span></span>

<span data-ttu-id="ff2e2-132">**V:** nemrég váltott egy prémium verzió (beleértve a rendszer próbaverzióját), ha látható adatok mentése 7 napra kezdetben.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-132">**A:** If you have recently switched to a Premium version (including a trial version), you can see data up to 7 days initially.</span></span> <span data-ttu-id="ff2e2-133">Összesít adatokat, ha akár 30 napig jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-133">When data accumulates, you will see up to 30 days.</span></span>

---

<span data-ttu-id="ff2e2-134">**K: van a kockázati események azonosító adatok védelmét, de nem látható az megfelelő bejelentkezhet az összes bejelentkezéseket. Ez várható?**</span><span class="sxs-lookup"><span data-stu-id="ff2e2-134">**Q: There is a risk event in Identity Protection but I’m not seeing corresponding sign-in in the all sign-ins. Is this expected?**</span></span>

<span data-ttu-id="ff2e2-135">**V:** Igen, Identity Protection kiértékeli minden hitelesítési forgalom kockázatát e ha interaktív vagy nem interaktív.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-135">**A:** Yes, Identity Protection evaluates risk for all authentication flows whether if be interactive or non-interactive.</span></span> <span data-ttu-id="ff2e2-136">Azonban minden bejelentkezések csak jelentésben csak a interaktív bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-136">However, all sign-ins only report shows only the interactive sign-ins.</span></span>

---

<span data-ttu-id="ff2e2-137">**K: hogyan töltheti Azure-portálon a "Kockázat megjelölt felhasználók" jelentést?**</span><span class="sxs-lookup"><span data-stu-id="ff2e2-137">**Q: How can I download the “Users flagged for risk” report in Azure portal?**</span></span>

<span data-ttu-id="ff2e2-138">**V:** letöltése beállítás *felhasználók meg van jelölve, a kockázat* jelentés hamarosan megjelenik.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-138">**A:** The option to download *Users flagged for risk* report will be added soon.</span></span>

---

<span data-ttu-id="ff2e2-139">**K: Hogyan állapítható meg, hogy miért a bejelentkezés vagy a felhasználó lett megjelölve kockázatos Azure-portálon?**</span><span class="sxs-lookup"><span data-stu-id="ff2e2-139">**Q: How do I know why a sign-in or a user was flagged risky in the Azure portal?**</span></span>

<span data-ttu-id="ff2e2-140">**V:** Premium edition ügyfelek tudhat meg többet az alapul szolgáló kockázati események a felhasználó a "Kockázat megjelölt felhasználók" vagy a "kockázatos bejelentkezések" parancsával.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-140">**A:** Premium edition customers can learn more about the underlying risk events by clicking on the user in “Users flagged for risk” or by clicking on the “Risky sign-ins”.</span></span> <span data-ttu-id="ff2e2-141">Szabad és alapvető edition ügyfelek eléréséhez at-risk felhasználók és bejelentkezések nélkül az alapul szolgáló kockázati események adatait.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-141">Free and Basic edition customers get to see the at-risk users and sign-ins without the underlying risk event information.</span></span>

---

<span data-ttu-id="ff2e2-142">**K: hogyan kiszámítása a bejelentkezéseket és a kockázatos bejelentkezések jelentés IP-címek??**</span><span class="sxs-lookup"><span data-stu-id="ff2e2-142">**Q: How are IP addresses calculated in the sign-ins and risky sign-ins report??**</span></span>

<span data-ttu-id="ff2e2-143">**V:** IP-címeket úgy, hogy nincs-e végleges kapcsolat közötti IP-cím és a számítógépre, hogy a cím fizikailag helyét adják.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-143">**A:** IP addresses are issued in such a way that there is no definitive connection between an IP address and where the computer with that address is physically located.</span></span> <span data-ttu-id="ff2e2-144">Ez például a mobil szolgáltatók és a kiállító központi készletek IP-címek nagyon gyakran távol, amelyekben ténylegesen szerepel az ügyféleszköz VPN tényezőktől bonyolítja.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-144">This is complicated by factors such as mobile providers and VPNs issuing IP addresses from central pools often very far from where the client device is actually used.</span></span> <span data-ttu-id="ff2e2-145">A fentiek IP-cím átalakítása a fizikai hely a nyomkövetési adatokat, beállításjegyzék-adatok, fordított keresések és egyéb információk alapján a lehető legkedvezőbb módon.</span><span class="sxs-lookup"><span data-stu-id="ff2e2-145">Given the above, converting IP address to a physical location is a best effort based on traces, registry data, reverse look ups and other information.</span></span> 

---
