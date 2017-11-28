---
title: "Több hiba után indított bejelentkezések"
description: "Ez a jelentés azt jelzi, felhasználók, akik a sikeres bejelentkezést követően több egymást követő sikertelen bejelentkezési kísérlet után."
services: active-directory
documentationcenter: 
author: SSalahAhmed
manager: femila
editor: 
ms.assetid: e4ec1a39-9c20-418f-8a75-6497d0117176
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/04/2016
ms.author: saah;kenhoff
ms.openlocfilehash: e55e0145adbdb1f41a8b8753d5555f20e96bf161
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="sign-ins-after-multiple-failures"></a><span data-ttu-id="a6dd9-103">Több hibát követő bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="a6dd9-103">Sign-ins after multiple failures</span></span>
<span data-ttu-id="a6dd9-104">Ez a jelentés azt jelzi, felhasználók, akik a sikeres bejelentkezést követően több egymást követő sikertelen bejelentkezési kísérlet után.</span><span class="sxs-lookup"><span data-stu-id="a6dd9-104">This report indicates users who have successfully signed in after multiple consecutive failed sign in attempts.</span></span> <span data-ttu-id="a6dd9-105">Lehetséges okok a következők:</span><span class="sxs-lookup"><span data-stu-id="a6dd9-105">Possible causes include:</span></span>

* <span data-ttu-id="a6dd9-106">Felhasználói kellett elfelejti a jelszavát</span><span class="sxs-lookup"><span data-stu-id="a6dd9-106">User had forgotten their password</span></span></li><li><span data-ttu-id="a6dd9-107">Felhasználó az áldozat sikeres jelszó-előállító találgatásos támadás kényszerítése</span><span class="sxs-lookup"><span data-stu-id="a6dd9-107">User is the victim of a successful password guessing brute force attack</span></span>

<span data-ttu-id="a6dd9-108">Ez a jelentés eredményeinek megtudhatja, hány egymást követő sikertelen bejelentkezési kísérlete sikeres bejelentkezés előtt készített és az első sikeres bejelentkezés társított időbélyeg.</span><span class="sxs-lookup"><span data-stu-id="a6dd9-108">Results from this report will show you the number of consecutive failed sign-in attempts made prior to the successful sign-in and a timestamp associated with the first successful sign-in.</span></span>

<span data-ttu-id="a6dd9-109">**Beállítások jelentés**: a lehető legkevesebb egymást követő sikertelen bejelentkezési kísérletek kell a jelentés megjelenítése előtt konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="a6dd9-109">**Report Settings**: You can configure the minimum number of consecutive failed sign in attempts that must occur before it can be displayed in the report.</span></span> <span data-ttu-id="a6dd9-110">Amikor módosítja ezt a beállítást fontos megjegyezni, hogy ezeket a módosításokat nem alkalmazandó összes meglévő sikertelen bejelentkezési modulok jelenleg jelenik meg a meglévő jelentés.</span><span class="sxs-lookup"><span data-stu-id="a6dd9-110">When you make changes to this setting it is important to note that these changes will not be applied to any existing failed sign ins that currently show up in your existing report.</span></span> <span data-ttu-id="a6dd9-111">Azonban akkor fog alkalmazni, az összes későbbi bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="a6dd9-111">However, they will be applied to all future sign-ins.</span></span> <span data-ttu-id="a6dd9-112">Ez a jelentés módosításai csak licenccel rendelkező rendszergazdák is végezhető.</span><span class="sxs-lookup"><span data-stu-id="a6dd9-112">Changes to this report can only be made by licensed admins.</span></span>

![Több hiba után indított bejelentkezések](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)

