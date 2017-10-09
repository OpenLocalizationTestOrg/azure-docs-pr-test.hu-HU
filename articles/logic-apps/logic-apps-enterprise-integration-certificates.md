---
title: "Vállalati integrációs csomag aaaUsing tanúsítványok |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse tanúsítványokat a vállalati integrációs csomag hello |} Az Azure Logic Apps alkalmazások"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: cgronlun
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 7ba9f597a03a852a9ba05d2af08fe4bc2d98aef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>A tanúsítványok és az Enterprise Integration Pack ismertetése
## <a name="overview"></a>Áttekintés
Vállalati integrációs tanúsítványok toosecure B2B kommunikációhoz használ. A vállalati integrációs alkalmazások két típusú tanúsítványt használhatja:

* Nyilvános tanúsítványokat, amelyek egy hitelesítésszolgáltatótól (CA) kell megvásárolni.
* Személyes tanúsítványok, amelyek saját kezűleg adhat ki. Ezek a tanúsítványok néha hivatkozott tooas önaláírt tanúsítványokat is.

## <a name="what-are-certificates"></a>Mik azok a tanúsítványok?
Tanúsítványok olyan digitális dokumentumok, ellenőrizze, hogy hello azonosságát hello résztvevők elektronikus kommunikáció, majd, amely is biztonságos elektronikus kommunikáció.

## <a name="why-use-certificates"></a>Tanúsítványok miért érdemes használni?
Egyes esetekben B2B kommunikációs kell tartani bizalmas. Vállalati integrációs használ a tanúsítványok toosecure ezek a kommunikációk két módon:

* Az üzenetek hello tartalma titkosításával
* Üzenetek digitális aláírásával  

## <a name="upload-a-public-certificate"></a>Nyilvános tanúsítvány feltöltése

toouse egy *nyilvános tanúsítvány* a logic apps B2B képességeket, először kell tooupload hello tanúsítvány integrációs fiókjába.  

A tanúsítvány feltöltése után-e a B2B üzenetek biztonságos, hello tulajdonságaik definiálásakor elérhető toohelp [megállapodások](logic-apps-enterprise-integration-agreements.md) az Ön által létrehozott.  

Az alábbiakban a hello feltöltése a nyilvános tanúsítványokat integrációs fiókjába, Azure-portálon toohello bejelentkezés után részletes lépései:

1. Válassza ki **további szolgáltatások** , és írja be **integrációs** hello szűrő Keresés mezőbe. Válassza ki **integrációs fiókok** a hello eredményeinek listája     
![Válassza ki a Tallózás gombra](media/logic-apps-enterprise-integration-certificates/overview-1.png)  
2. Válassza ki a hello integrációs fiók toowhich szeretne tooadd hello tanúsítványt.  
![Válassza ki a hello integrációs fiók toowhich szeretne tooadd hello tanúsítványt](media/logic-apps-enterprise-integration-certificates/overview-3.png)  
3. Jelölje be hello **tanúsítványok** csempére.  
![Jelölje be hello tanúsítványok csempe](media/logic-apps-enterprise-integration-certificates/certificate-1.png)
4. A hello **tanúsítványok** panelt megnyitó, válassza hello **Hozzáadás** gombra.   
![Válassza ki a hello Hozzáadás gomb](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
5. Adjon meg egy **neve** a tanúsítványt, és jelölje ki hello tanúsítvány típusú, mint **nyilvános** hello legördülő menüből.  
6. Hello hello jobb oldalán válassza hello ikonja **tanúsítvány** szövegmezőben. Hello fájlválasztó megnyitása, keresse meg és jelölje ki a megjeleníteni kívánt tooupload tooyour integrációs fiók hello tanúsítványfájlt.
7. Hello tanúsítványt, majd válassza ki és **OK** a hello fájlválasztó. Ez érvényesíti, és feltölti a hello tanúsítvány tooyour integrációs fiók.
8. Végezetül biztonsági hello **Hozzáadás tanúsítvány** panelen, jelölje be hello **OK** gombra.  
![Válassza ki a hello OK gomb](media/logic-apps-enterprise-integration-certificates/certificate-3.png)  
9. Jelölje be hello **tanúsítványok** csempére. Meg kell jelennie a hello hozzáadta az új tanúsítványt.  
![Tekintse meg az új tanúsítvány hello](media/logic-apps-enterprise-integration-certificates/certificate-4.png)  

## <a name="upload-a-private-certificate"></a>Személyes tanúsítvány feltöltése

toouse egy *személyes tanúsítvány* a logic apps B2B képességeket, a feltölthet egy személyes tanúsítvány tooyour integrációs fiók véve hello lépések

1. [Töltse fel a titkos kulcs tooKey tároló](../key-vault/key-vault-get-started.md "további információ a Key Vault") , és adjon meg egy **kulcs neve** 
   
   > [!TIP]
   > Engedélyeznie kell a Key Vault Logic Apps tooperform műveletek. Hozzáférés toohello Logic Apps szolgáltatás egyszerű hello a következő PowerShell-parancs használatával adja meg:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  
   > 
   > 

Miután hello előző lépést, a személyes tanúsítvány toointegration fiók hozzáadása.

Következő vannak hello részletes, lépésenkénti leírását a személyes tanúsítványok feltöltése a integrációs fiókjába, a bejelentkezést toohello Azure-portálon:  
 
1. Válassza ki a hello integrációs fiók toowhich szeretné, hogy tooadd hello tanúsítványt, és válassza ki a hello **tanúsítványok** csempére.  
![Jelölje be hello tanúsítványok csempe](media/logic-apps-enterprise-integration-certificates/certificate-1.png)  
2. A hello **tanúsítványok** panelt megnyitó, válassza hello **Hozzáadás** gombra.   
![Válassza ki a hello Hozzáadás gomb](media/logic-apps-enterprise-integration-certificates/certificate-2.png)
3. Adjon meg egy **neve** a tanúsítványt, és jelölje be hello tanúsítvány típusú, mint **titkos** hello legördülő menüből.   
4. Válassza ki a hello ikonja hello jobb oldalán hello **tanúsítvány** szövegmezőben. Hello fájlválasztó megnyitása után hello megfelelő nyilvános tanúsítvány, amelyet az tooupload tooyour integrációs fiók található.   
   
   > [!Note]
   > Személyes tanúsítvány hozzáadása során fontos, megfelelő nyilvános tooadd tanúsítvány a tooshow [AS2 megállapodás](logic-apps-enterprise-integration-as2.md) beállítások aláírásához és titkosításához üdvözlő üzenetek küldésére és fogadására.
   > 
   >   

5. Jelölje be hello **erőforráscsoport**, **Key Vault**, **kulcsnév** és select hello **OK** gombra.  
![Tanúsítvány hozzáadása](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)  
6. Jelölje be hello **tanúsítványok** csempére. Meg kell jelennie a hello hozzáadta az új tanúsítványt.
![Tekintse meg az új tanúsítvány hello](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)  



* [B2B-egyezmény létrehozása](logic-apps-enterprise-integration-agreements.md)  
* [További információ a Key Vault](../key-vault/key-vault-get-started.md "Key Vault megismerése")  

