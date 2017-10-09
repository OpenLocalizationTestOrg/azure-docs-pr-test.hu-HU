---
title: "a klasszikus Azure portálon hello aaaKnown hálózatok |} Microsoft Docs"
description: "Ismert hálózatok konfigurálásával elkerülheti, hogy hello bejelentkezési különböző földrajzi régiókból és bejelentkezési modulokat IP-címekről a gyanús tevékenység jelentések szerepel a szervezete tulajdonában lévő IP-címek."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: ec636cdda172cd3baeb1e606dd8d6e1949fbc63b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="known-networks"></a><span data-ttu-id="ef03f-103">Ismert hálózatok</span><span class="sxs-lookup"><span data-stu-id="ef03f-103">Known networks</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="ef03f-104">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="ef03f-104">Azure classic portal</span></span>](active-directory-known-networks.md)
> * [<span data-ttu-id="ef03f-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ef03f-105">Azure portal</span></span>](active-directory-known-networks-azure-portal.md)
> 
> 


<span data-ttu-id="ef03f-106">Használhatja az Azure Active Directory hozzáférési és használati jelentések toogain láthatósága hello adatintegritási és biztonsági a szervezete címtárát.</span><span class="sxs-lookup"><span data-stu-id="ef03f-106">You can use Azure Active Directory's access and usage reports toogain visibility into hello integrity and security of your organization’s directory.</span></span> <span data-ttu-id="ef03f-107">Ezt az információt a directory-rendszergazda is jobban meghatározhatja, ahol lehetséges biztonsági kockázatokat, hogy azok megfelelően megtervezheti toomitigate kockázatok vizsgálandó.</span><span class="sxs-lookup"><span data-stu-id="ef03f-107">With this information, a directory admin can better determine where possible security risks may lie so that they can adequately plan toomitigate those risks.</span></span>

<span data-ttu-id="ef03f-108">Lehetséges, hogy a hello "*több földrajzi területről indított bejelentkezések*"és"*IP-címekről a gyanús tevékenységeket bejelentkezések*" jelentések helytelenül Ez a jelző ténylegesen tulajdonában lévő IP-címek a szervezet.</span><span class="sxs-lookup"><span data-stu-id="ef03f-108">It is possible that hello “*Sign ins from multiple geographies*” and “*Sign ins from IP addresses with suspicious activity*” reports incorrectly flag IP addresses that are actually owned by your organization.</span></span> 

<span data-ttu-id="ef03f-109">Ez történik például is, ha:</span><span class="sxs-lookup"><span data-stu-id="ef03f-109">This can, for example, happen when:</span></span> 

* <span data-ttu-id="ef03f-110">A felhasználó a Boston Office aláírta San Francisco eseményindítók hello "Különböző földrajzi régiókból bejelentkezési ins" jelentés távolról tooyour data Center</span><span class="sxs-lookup"><span data-stu-id="ef03f-110">A user in your Boston office has signed in remotely tooyour data center in San Francisco triggers hello “Sign ins from multiple geographies” report</span></span> 
* <span data-ttu-id="ef03f-111">A szervezet egy felhasználó megpróbál toosign-meg többször a egy helytelen jelszót eseményindítók hello "IP-címekről a gyanús tevékenységeket bejelentkezési ins" jelentés</span><span class="sxs-lookup"><span data-stu-id="ef03f-111">A user of your organization tries toosign-on several times with an incorrect password triggers hello “Sign ins from IP addresses with suspicious activity” report</span></span> 

<span data-ttu-id="ef03f-112">tooprevent ezekben az esetekben a előállítása félrevezető biztonsági jelentések, ismert IP-címtartományok toohello címlista a vállalat nyilvános IP-cím hozzá kell adnia.</span><span class="sxs-lookup"><span data-stu-id="ef03f-112">tooprevent these cases from generating misleading security reports, you should add known IP address ranges toohello list of your organization's public IP address.</span></span>    

### <a name="tooadd-your-organizations-public-ip-address-ranges-perform-hello-following-steps"></a><span data-ttu-id="ef03f-113">tooadd a vállalat nyilvános IP-címtartományt, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="ef03f-113">tooadd your organization’s public IP address ranges, perform hello following steps:</span></span>

1. <span data-ttu-id="ef03f-114">Bejelentkezés toohello [Azure felügyeleti portálján](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="ef03f-114">Sign-on toohello [Azure management portal](https://manage.windowsazure.com).</span></span>

2. <span data-ttu-id="ef03f-115">Hello bal oldali ablaktáblában kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="ef03f-115">In hello left pane, click **Active Directory**.</span></span> 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-01.png)

3. <span data-ttu-id="ef03f-117">A hello **Directory** lapra, válassza ki azt a címtárat.</span><span class="sxs-lookup"><span data-stu-id="ef03f-117">In hello **Directory** tab, select your directory.</span></span>

4. <span data-ttu-id="ef03f-118">Hello hello felső menüben kattintson a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="ef03f-118">In hello menu on hello top, click **Configure**.</span></span> 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-02.png)

5. <span data-ttu-id="ef03f-120">Hello konfigurálása lapon lépjen túl**a szervezetek nyilvános IP-címtartományok**</span><span class="sxs-lookup"><span data-stu-id="ef03f-120">On hello Configure tab, go too**your organizations public IP address ranges**</span></span> 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-03.png)

6. <span data-ttu-id="ef03f-122">Kattintson a **adja hozzá az ismert IP-címtartományok**.</span><span class="sxs-lookup"><span data-stu-id="ef03f-122">Click **Add Known IP Address Ranges**.</span></span>

7. <span data-ttu-id="ef03f-123">Adja hozzá a címtartomány hello párbeszédpanelt, amely akkor jelenik meg, és kattintson hello gombot, amikor elkészült.</span><span class="sxs-lookup"><span data-stu-id="ef03f-123">Add your address ranges in hello dialog box that appears, and then click hello check button  when you are done.</span></span> 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-04.png)

<span data-ttu-id="ef03f-125">**További források:**</span><span class="sxs-lookup"><span data-stu-id="ef03f-125">**Additional resources:**</span></span>

* [<span data-ttu-id="ef03f-126">A hozzáférési és használati jelentések megtekintése</span><span class="sxs-lookup"><span data-stu-id="ef03f-126">View your access and usage reports</span></span>](active-directory-view-access-usage-reports.md)
* [<span data-ttu-id="ef03f-127">IP-címekről a gyanús tevékenységeket bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="ef03f-127">Sign ins from IP addresses with suspicious activity</span></span>](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [<span data-ttu-id="ef03f-128">Több földrajzi területről indított bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="ef03f-128">Sign ins from multiple geographies</span></span>](active-directory-reporting-sign-ins-from-multiple-geographies.md)

