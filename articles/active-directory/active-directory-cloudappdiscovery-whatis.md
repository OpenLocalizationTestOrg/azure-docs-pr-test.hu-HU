---
title: "aaaFinding nem felügyelt felhőalapú alkalmazásokhoz, a Cloud App Discovery |} Microsoft Docs"
description: "Információk keresése és alkalmazások kezelése az a Cloud App Discovery, melyek hello előnyei és annak működéséről."
services: active-directory
keywords: "a cloud app discovery alkalmazások kezelése"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 50c24af9bb400e4be11f4ad2d1de13d26f5467bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="finding-unmanaged-cloud-applications-with-cloud-app-discovery"></a>A Cloud App Discovery megállapítás nem felügyelt felhőalkalmazások
## <a name="overview"></a>Áttekintés
A modern vállalatok informatikai részlegek ismerik gyakran nem minden hello felhőalapú alkalmazásokhoz, hogy a szervezet tagjai használjon toodo munkavégzést. Miért a rendszergazdák jogosulatlan hozzáférés toocorporate adatokat, a lehetséges adatszivárgás és az egyéb biztonsági kockázatok kétségei vannak könnyen toosee. Tájékoztatási hiánya teheti a felsorolt biztonsági kockázatok kezelésével tűnhet terv létrehozása.

A cloud App Discovery lehetővé teszi az Azure Active Directory (AD) támogatás, amely lehetővé teszi a szervezet hello dolgozói által használt toodiscover felhőalapú alkalmazásokhoz.

**A Cloud App Discovery a következőket teheti:**

* Keresse meg használt hello felhőalapú alkalmazásokhoz, és mérheti, hogy a használatát azon felhasználók számát, a forgalom vagy a kérelmek toohello webalkalmazás száma.
* Az alkalmazás által használt hello felhasználók azonosítása.
* Exportálhatja az adatokat kapcsolat nélküli elemzéshez.
* Ezek az alkalmazások informatikai vezérlése alatt állapotba, és engedélyezze a egyszeri bejelentkezéshez a felhasználókezeléshez.

## <a name="how-it-works"></a>Működés
1. Alkalmazás használati ügynök telepítve van a felhasználók számítógépeire.
2. hello alkalmazás használati adatai hello-ügynökök által rögzített egy biztonságos, titkosított csatornát felderítési toohello felhőalapú alkalmazásszolgáltatás továbbítása.
3. a Cloud App Discovery szolgáltatás hello hello adatok kiértékeli, és létrehozza a jelentéseket.

![Cloud App Discovery diagramja](./media/active-directory-cloudappdiscovery/cad01.png)

lépések a Cloud App Discovery tooget lásd [első lépések a Cloud App Discovery szolgáltatásra](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Cloud App Discovery biztonsági és adatvédelmi megfontolások](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [Cloud App Discovery csoport házirendjének telepítési útmutatója](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)
* [Cloud App Discovery a System Center telepítési útmutatója](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)
* [Cloud App Discovery beállításjegyzék-beállítások egyéni porttal proxykiszolgálók](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [Cloud App Discovery-ügynök változásnaplója](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [A cloud App Discovery gyakran ismételt kérdések](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)

