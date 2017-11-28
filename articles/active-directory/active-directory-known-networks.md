---
title: "Hálózatok ismert a klasszikus Azure portálon |} Microsoft Docs"
description: "Ismert hálózatok konfigurálásával elkerülheti, ha szerepel a bejelentkezési különböző földrajzi régiókból és bejelentkezési modulokat IP-címekről a gyanús tevékenység jelentések a szervezet tulajdonában lévő IP-címek."
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
ms.openlocfilehash: e4d51d1d2f09fca34d749879e21d49f785eac35c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="known-networks"></a><span data-ttu-id="2c4f4-103">Ismert hálózatok</span><span class="sxs-lookup"><span data-stu-id="2c4f4-103">Known networks</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2c4f4-104">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="2c4f4-104">Azure classic portal</span></span>](active-directory-known-networks.md)
> * [<span data-ttu-id="2c4f4-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="2c4f4-105">Azure portal</span></span>](active-directory-known-networks-azure-portal.md)
> 
> 


<span data-ttu-id="2c4f4-106">Azure Active Directory hozzáférési és használati jelentések segítségével hogy lássák az integritásra és a munkahely címtárában biztonságát.</span><span class="sxs-lookup"><span data-stu-id="2c4f4-106">You can use Azure Active Directory's access and usage reports to gain visibility into the integrity and security of your organization’s directory.</span></span> <span data-ttu-id="2c4f4-107">Ezt az információt a directory-rendszergazda is jobban meghatározhatja, ahol lehetséges biztonsági kockázatokat a vizsgálandó, hogy azok megfelelően megtervezheti kockázatok csökkentésének lehetőségeit.</span><span class="sxs-lookup"><span data-stu-id="2c4f4-107">With this information, a directory admin can better determine where possible security risks may lie so that they can adequately plan to mitigate those risks.</span></span>

<span data-ttu-id="2c4f4-108">Lehetséges, hogy a "*több földrajzi területről indított bejelentkezések*"és"*IP-címekről a gyanús tevékenységeket bejelentkezések*" jelentések helytelenül jelzőt a szervezet ténylegesen tulajdonában lévő IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="2c4f4-108">It is possible that the “*Sign ins from multiple geographies*” and “*Sign ins from IP addresses with suspicious activity*” reports incorrectly flag IP addresses that are actually owned by your organization.</span></span> 

<span data-ttu-id="2c4f4-109">Ez történik például is, ha:</span><span class="sxs-lookup"><span data-stu-id="2c4f4-109">This can, for example, happen when:</span></span> 

* <span data-ttu-id="2c4f4-110">Az office bejelentkezett az távolról az adatközpont, a San Francisco Boston egy felhasználó elindítja a "Bejelentkezések különböző földrajzi régiókból" jelentés</span><span class="sxs-lookup"><span data-stu-id="2c4f4-110">A user in your Boston office has signed in remotely to your data center in San Francisco triggers the “Sign ins from multiple geographies” report</span></span> 
* <span data-ttu-id="2c4f4-111">A felhasználó a szervezet próbál jelentkezhessen be többször egy helytelen jelszót eseményindítók az "IP-címekről a gyanús tevékenységeket bejelentkezések" jelentés</span><span class="sxs-lookup"><span data-stu-id="2c4f4-111">A user of your organization tries to sign-on several times with an incorrect password triggers the “Sign ins from IP addresses with suspicious activity” report</span></span> 

<span data-ttu-id="2c4f4-112">Ezekben az esetekben félrevezető biztonsági jelentések létrehozása érdekében ismert IP-címtartományok kell hozzáadása a listához, a vállalat nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="2c4f4-112">To prevent these cases from generating misleading security reports, you should add known IP address ranges to the list of your organization's public IP address.</span></span>    

### <a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a><span data-ttu-id="2c4f4-113">Adja hozzá a szervezet nyilvános IP-címtartományok, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2c4f4-113">To add your organization’s public IP address ranges, perform the following steps:</span></span>

1. <span data-ttu-id="2c4f4-114">Bejelentkezés a [Azure felügyeleti portálján](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="2c4f4-114">Sign-on to the [Azure management portal](https://manage.windowsazure.com).</span></span>

2. <span data-ttu-id="2c4f4-115">Kattintson a bal oldali ablaktáblában **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="2c4f4-115">In the left pane, click **Active Directory**.</span></span> 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-01.png)

3. <span data-ttu-id="2c4f4-117">Az a **Directory** lapra, válassza ki azt a címtárat.</span><span class="sxs-lookup"><span data-stu-id="2c4f4-117">In the **Directory** tab, select your directory.</span></span>

4. <span data-ttu-id="2c4f4-118">Kattintson a felső menüben **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="2c4f4-118">In the menu on the top, click **Configure**.</span></span> 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-02.png)

5. <span data-ttu-id="2c4f4-120">Lépjen a konfigurálása lapon **a szervezetek nyilvános IP-címtartományok**</span><span class="sxs-lookup"><span data-stu-id="2c4f4-120">On the Configure tab, go to **your organizations public IP address ranges**</span></span> 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-03.png)

6. <span data-ttu-id="2c4f4-122">Kattintson a **adja hozzá az ismert IP-címtartományok**.</span><span class="sxs-lookup"><span data-stu-id="2c4f4-122">Click **Add Known IP Address Ranges**.</span></span>

7. <span data-ttu-id="2c4f4-123">Adja hozzá a címtartomány a megjelenő párbeszédpanelen, és amikor elkészült, kattintson a pipa gombra.</span><span class="sxs-lookup"><span data-stu-id="2c4f4-123">Add your address ranges in the dialog box that appears, and then click the check button  when you are done.</span></span> 

    ![Ismert hálózatok](./media/active-directory-known-networks/known-netwoks-04.png)

<span data-ttu-id="2c4f4-125">**További források:**</span><span class="sxs-lookup"><span data-stu-id="2c4f4-125">**Additional resources:**</span></span>

* [<span data-ttu-id="2c4f4-126">A hozzáférési és használati jelentések megtekintése</span><span class="sxs-lookup"><span data-stu-id="2c4f4-126">View your access and usage reports</span></span>](active-directory-view-access-usage-reports.md)
* [<span data-ttu-id="2c4f4-127">IP-címekről a gyanús tevékenységeket bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="2c4f4-127">Sign ins from IP addresses with suspicious activity</span></span>](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [<span data-ttu-id="2c4f4-128">Több földrajzi területről indított bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="2c4f4-128">Sign ins from multiple geographies</span></span>](active-directory-reporting-sign-ins-from-multiple-geographies.md)

