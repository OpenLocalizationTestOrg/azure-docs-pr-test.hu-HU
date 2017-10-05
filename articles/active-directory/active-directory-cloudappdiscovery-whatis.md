---
title: "Nem felügyelt felhőalapú alkalmazásokhoz, a Cloud App Discovery keresése |} Microsoft Docs"
description: "Információk keresése és alkalmazások kezelése az a Cloud App Discovery, milyen előnyökkel és annak működéséről."
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
ms.openlocfilehash: 6284ff5bac8edbc19561d0916adef153526dfbe3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="finding-unmanaged-cloud-applications-with-cloud-app-discovery"></a>A Cloud App Discovery megállapítás nem felügyelt felhőalkalmazások
## <a name="overview"></a>Áttekintés
A modern vállalatok informatikai részlegek ismerik gyakran nem a szervezet tagjai segítségével végezhesse a munkáját felhőalapú alkalmazásokhoz. Legyen könnyen látható, hogy miért a rendszergazdák jogosulatlan hozzáférés a vállalati adatokat, lehetséges adatszivárgás és egyéb biztonsági kockázatok kétségei vannak. Tájékoztatási hiánya teheti a felsorolt biztonsági kockázatok kezelésével tűnhet terv létrehozása.

A cloud App Discovery lehetővé teszi az Azure Active Directory (AD) támogatás, amely lehetővé teszi, hogy a szervezet dolgozói által használt felhőalkalmazások felderítése.

**A Cloud App Discovery a következőket teheti:**

* Keresse meg a felhőalapú alkalmazások használják, és mérheti a használatot felhasználók száma, a forgalom vagy a webkiszolgáló az alkalmazásnak küldött kérelmek száma.
* Azonosítsa az adott alkalmazást használó felhasználókat.
* Exportálhatja az adatokat kapcsolat nélküli elemzéshez.
* Ezek az alkalmazások informatikai vezérlése alatt állapotba, és engedélyezze a egyszeri bejelentkezéshez a felhasználókezeléshez.

## <a name="how-it-works"></a>Működés
1. Alkalmazás használati ügynök telepítve van a felhasználók számítógépeire.
2. Az alkalmazás használati adatait, az ügynökök által rögzített a cloud app discovery szolgáltatásba zajlik a biztonságos, titkosított csatornán.
3. A Cloud App Discovery szolgáltatásba kiértékeli az adatokat, és létrehozza a jelentéseket.

![Cloud App Discovery diagramja](./media/active-directory-cloudappdiscovery/cad01.png)

Első lépések a Cloud App Discovery, lásd: [első lépések a Cloud App Discovery szolgáltatásra](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)

## <a name="related-articles"></a>Kapcsolódó cikkek
* [Cloud App Discovery biztonsági és adatvédelmi megfontolások](active-directory-cloudappdiscovery-security-and-privacy-considerations.md)  
* [Cloud App Discovery csoport házirendjének telepítési útmutatója](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx)
* [Cloud App Discovery a System Center telepítési útmutatója](http://social.technet.microsoft.com/wiki/contents/articles/30968.cloud-app-discovery-system-center-deployment-guide.aspx)
* [Cloud App Discovery beállításjegyzék-beállítások egyéni porttal proxykiszolgálók](active-directory-cloudappdiscovery-registry-settings-for-proxy-services.md)
* [Cloud App Discovery-ügynök változásnaplója](http://social.technet.microsoft.com/wiki/contents/articles/24616.cloud-app-discovery-agent-changelog.aspx)
* [A cloud App Discovery gyakran ismételt kérdések](http://social.technet.microsoft.com/wiki/contents/articles/24037.cloud-app-discovery-frequently-asked-questions.aspx)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)

