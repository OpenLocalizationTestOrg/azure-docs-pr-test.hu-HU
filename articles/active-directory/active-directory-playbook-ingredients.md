---
title: "Active Directory koncepció alkalmazástervezési összetevők aaaAzure |} Microsoft Docs"
description: "Vizsgálatát, és gyorsan végrehajthatja az identitás- és kezelési helyzetek"
services: active-directory
keywords: "az Azure active directory, a forgatókönyv, a koncepció igazolása, PoC"
documentationcenter: 
author: dstefanMSFT
manager: asuthar
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/12/2017
ms.author: dstefan
ms.openlocfilehash: 0a7f5cd659b9d62ac86e3c27e5727294d481f4a2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-proof-of-concept-playbook-ingredients"></a>Az Azure Active Directory igazolása koncepció alkalmazástervezési összetevők 

## <a name="theme"></a>Téma
Az Azure AD lehetővé teszi az identitás- és hozzáférés megoldások hello vállalat több területet. A következő területeken hello hello forgatókönyvei azt besorolása: 

* [Nagy mennyiségű alkalmazásokat, egy identitás](active-directory-playbook-implementation.md#theme---lots-of-apps-one-identity) 
* [A biztonság növelése](active-directory-playbook-implementation.md#theme---increase-your-security) 
* [Méretezéssel önkiszolgáló,](active-directory-playbook-implementation.md#theme---scale-with-self-service) 

Határozza meg a téma tooframe hello koncepció toofocus hello erőfeszítéseket segít az üzleti céljaihoz mellőzése, amelyek gyakran hello eseményindítók a koncepció igazolása hello érdeklő hello első hely. 

## <a name="environment"></a>Környezet

Fontos fontos toodetermine hello részletek hello környezet, amelyben hello koncepció fog továbbítani. Ideális esetben hozhat létre léphessen hello koncepció befejeződése után. hello célkörnyezet elengedhetetlen, és keresse meg azt a valódi lehető és megkötések vagy további szempontok hello terhek közötti hello egyensúlyt. tipikus környezeteket hello POC-khez használható a következők:
* **Éles:** hello forgatókönyvek hajtják végre élő környezetében, és már központilag telepítve a Microsoft Cloud services (éles AD, Office 365, az Azure AD bérlő/SSO megoldás). 
* **Felhasználói elfogadás tesztelése ellenőrzését / fejlesztői környezetben:** teszt infrastruktúrával rendelkezik (párhuzamos AD és az esetlegesen Azure AD bérlő/SSO megoldás) a Tesztadatok levő üzemi. Általában hello tesztkörnyezetben által megosztott több projektet hello vállalaton belül.

Az útmutatóban a legtöbb forgatókönyv additívak jellegűek. Ennek eredményeképpen ezek telepíthető hello éles bérlői hello koncepció kívüli felhasználók befolyásolása nélkül. Ez a dokumentum azt fogja kell létesítő változatok hatása lenne bérlői szintű. Ezekben az esetekben érdemes lehet tooconsider nem éles környezetben. 


## <a name="target-users"></a>Azon felhasználók megcélzása

Fontos toodetermine hello célkészlet, amely intéz a hello forgatókönyvek, különösen ha hello környezet üzemi és tesztkörnyezetben is. a koncepció azon felhasználók megcélzása hello kategóriái a következők:
* **Próbafelhasználók:** ellátására használnak, a nap tooday hello fiókkal hello megoldással hello környezetben valós felhasználói feladat-funkciók
* **Tesztfelhasználók:** hello környezetben létrehozott fiókot tesztelése 

Ez az útmutató a legtöbb esetben kísérleti felhasználók lehet érvényesíteni. Ez a dokumentum azt fogja kell létesítő cél felhasználói kapcsolatos megfontolások szükség esetén.


[!INCLUDE [active-directory-playbook-toc](../../includes/active-directory-playbook-steps.md)]