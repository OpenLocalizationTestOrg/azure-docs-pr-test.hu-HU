---
title: "aaaSign modulok több hiba után"
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
ms.openlocfilehash: 48d137dc3abf65287cb3b9ba8a6ff10340f6741f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sign-ins-after-multiple-failures"></a><span data-ttu-id="2ca07-103">Több hibát követő bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="2ca07-103">Sign-ins after multiple failures</span></span>
<span data-ttu-id="2ca07-104">Ez a jelentés azt jelzi, felhasználók, akik a sikeres bejelentkezést követően több egymást követő sikertelen bejelentkezési kísérlet után.</span><span class="sxs-lookup"><span data-stu-id="2ca07-104">This report indicates users who have successfully signed in after multiple consecutive failed sign in attempts.</span></span> <span data-ttu-id="2ca07-105">Lehetséges okok a következők:</span><span class="sxs-lookup"><span data-stu-id="2ca07-105">Possible causes include:</span></span>

* <span data-ttu-id="2ca07-106">Felhasználói kellett elfelejti a jelszavát</span><span class="sxs-lookup"><span data-stu-id="2ca07-106">User had forgotten their password</span></span></li><li><span data-ttu-id="2ca07-107">Felhasználó hello áldozata sikeres jelszó-előállító találgatásos támadás</span><span class="sxs-lookup"><span data-stu-id="2ca07-107">User is hello victim of a successful password guessing brute force attack</span></span>

<span data-ttu-id="2ca07-108">Ez a jelentés eredményeinek jelennek meg, hány egymást követő sikertelen bejelentkezési kísérletek előzetes toohello sikeres bejelentkezés hello és társított időbélyeg hello első sikeres bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="2ca07-108">Results from this report will show you hello number of consecutive failed sign-in attempts made prior toohello successful sign-in and a timestamp associated with hello first successful sign-in.</span></span>

<span data-ttu-id="2ca07-109">**Beállítások jelentés**: hello minimális számú egymást követő sikertelen bejelentkezési kísérletek kell hello jelentés megjelenítése előtt konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="2ca07-109">**Report Settings**: You can configure hello minimum number of consecutive failed sign in attempts that must occur before it can be displayed in hello report.</span></span> <span data-ttu-id="2ca07-110">Állítaná toothis van módosításokat fontos toonote, hogy a módosítások nem lesznek alkalmazott tooany meglévő sikertelen bejelentkezések a meglévő jelentés jelenleg látható.</span><span class="sxs-lookup"><span data-stu-id="2ca07-110">When you make changes toothis setting it is important toonote that these changes will not be applied tooany existing failed sign ins that currently show up in your existing report.</span></span> <span data-ttu-id="2ca07-111">Azonban alkalmazott tooall későbbi bejelentkezések fogja. A licenccel rendelkező rendszergazdák csak is végezhető módosítások toothis jelentés.</span><span class="sxs-lookup"><span data-stu-id="2ca07-111">However, they will be applied tooall future sign-ins. Changes toothis report can only be made by licensed admins.</span></span>

![Több hiba után indított bejelentkezések](./media/active-directory-reporting-sign-ins-after-multiple-failures/signInsAfterMultipleFailures.PNG)

