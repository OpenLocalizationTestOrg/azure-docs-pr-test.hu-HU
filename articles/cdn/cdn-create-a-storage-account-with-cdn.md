---
title: "egy Azure storage-fiók Azure CDN aaaIntegrate |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure tartalom Delivery Network (CDN) toodeliver nagy sávszélességű tartalom gyorsítótárazása révén blobok Azure Storage-ból."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: cbc2ff98-916d-4339-8959-622823c5b772
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e44716969d6a784265cc4b1da34f0d021a17b38d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a>Azure-tárfiók integrálása az Azure CDN szolgáltatás használata
CDN lehet engedélyezve az Azure tárterületet lévő tartalmak toocache. A fejlesztők a tartalmak nagy sávszélességű kézbesítéséhez a blobok és számítási példányokért fizikai csomópontokon hello az Amerikai Egyesült Államok, Európa, Ázsia, Ausztrália és Dél-Amerika a statikus tartalom gyorsítótárazása révén globális megoldást kínál.

## <a name="step-1-create-a-storage-account"></a>1. lépés: Tárfiók létrehozása
A következő eljárás toocreate egy új tárfiókot, Azure-előfizetéshez tartozó hello használata. A storage-fiók az Azure storage-szolgáltatásokhoz való hozzáférést. hello tárfiók hello legmagasabb szintű hello névtér eléréséhez hello az Azure storage szolgáltatás összetevői jelöli: Blob-szolgáltatások, Queue szolgáltatások és Table szolgáltatások. További információkért tekintse meg a toohello [Azure Storage bemutatása tooMicrosoft](../storage/common/storage-introduction.md).

a tárfiók toocreate, kell lennie, vagy hello szolgáltatás rendszergazdájának vagy társadminisztrátorának a kapcsolódó hello előfizetés.

> [!NOTE]
> Többféleképpen is használhat egy tárfiókot, beleértve a hello Azure portál és a Powershell toocreate.  Ebben az oktatóanyagban használni fogjuk hello Azure portálon.  
> 
> 

**toocreate egy tárfiókot, Azure-előfizetések**

1. Jelentkezzen be toohello [Azure Portal](https://portal.azure.com).
2. Hello bal felső sarokban, válassza ki **új**. A hello **új** párbeszédpanelen válassza **adatok + tárolás**, majd kattintson a **tárfiók**.
    
    Hello **storage-fiók létrehozása** panel jelenik meg.   

    ![Storage-fiók létrehozása][create-new-storage-account]  

3. A hello **neve** mezőbe írja be egy altartomány nevét. Ez a bejegyzés 3-24 kisbetűket és számokat tartalmazhat.
   
    Ez az érték lesz hello állomásnév belül hello hello előfizetés Blob, sor vagy tábla erőforrásainak címzéséhez használt URI-Azonosítóra. A Blob szolgáltatás hello tároló erőforrás megoldására használt URI hello a következő formátumban, ahol  *&lt;StorageAccountLabel&gt;*  beírt toohello érték hivatkozik **URL-címetadjonmeg**:
   
    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*
   
    **Fontos:** hello URL-cím címke űrlapok hello altartomány hello storage-fiókjának URI és az Azure-ban az összes üzemeltetett szolgáltatások egyedinek kell lennie.
   
    Ez az érték is használható hello neveként a tárfiók hello portálon, vagy ennek a fióknak programozott módon való hozzáférés során.
4. Hagyja meg az alapértelmezett hello beállításait **telepítési modell**, **fiók kind**, **teljesítmény**, és **replikációs**. 
5. Jelölje be hello **előfizetés** , hogy hello tárfiók lesz használható.
6. Válasszon ki vagy hozzon létre egy **erőforráscsoportot**.  További információ az erőforráscsoportokkal kapcsolatban: [Az Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Válasszon egy helyet a tárfiók.
8. Kattintson a **Create** (Létrehozás) gombra. hello a hello storage-fiók létrehozása eltarthat néhány percig toocomplete.

## <a name="step-2-enable-cdn-for-hello-storage-account"></a>2. lépés: A hello tárfiók CDN engedélyezése

A legújabb integrációs hello most már engedélyezheti CDN a tárfiók a tárolási portálbővítményt maradjanak. 

1. Válassza ki hello tárfiókot, "CDN" vagy görgessen lefelé kereshet hello bal oldali navigációs menü, majd kattintson az "Azure CDN".
    
    Hello **Azure CDN** panel jelenik meg.

    ![CDN engedélyezése navigációs][cdn-enable-navigation]
    
2. Új végpont létrehozásához szükséges hello beírásával
    - **CDN-profil**: hozzon létre egy új, vagy egy meglévő profil.
    - **IP-címek**: csak akkor kell tooselect egy tarifacsomagra új CDN-profil létrehozásakor.
    - **CDN-végpont nevének**: Adja meg a választott / egy végpont nevét.

    > [!TIP]
    > CDN-végpont létrehozása hello származási hello állomásnév a tárfiók alapértelmezés szerint használja.

    ! [cdn új végpont létrehozásához] [a cdn-új-végpont-létrehozása]

3. A létrehozás után hello új végpont a fenti hello végpont listában jelennek meg.

    ![tárolási új CDN-végpont][cdn-storage-new-endpoint]

> [!NOTE]
> TooAzure CDN bővítmény tooenable CDN is választhatja. [Oktatóanyag](#Tutorial-cdn-create-profile).
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]  

## <a name="step-3-enable-additional-cdn-features"></a>3. lépés: További CDN-funkciók engedélyezése

A storage-fiók "Azure CDN" panelen kattintson az hello CDN-végpont hello lista tooopen CDN konfigurációs paneljén. További CDN szolgáltatásai engedélyezheti a kézbesítésre, például a tömörítés, lekérdezési karakterláncot, földrajzi szűrést. Adja hozzá az egyéni tartomány leképezése tooyour CDN-végpont is, és az egyéni tartomány HTTPS engedélyezése.
    
![CDN cdn tárolással][cdn-storage-cdn-configuration]

## <a name="step-4-access-cdn-content"></a>4. lépés: Hozzáférési CDN-tartalom
megadott tooaccess gyorsítótárba helyezték a tartalmat a hello CDN, a CDN URL-cím használata hello hello portálon. gyorsítótárazott blob hello címet hasonló toohello következő lesz:

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*Blobnév*\>

> [!NOTE]
> Ha engedélyezi a CDN hozzáférés tooa storage-fiók, az összes nyilvánosan elérhető objektumok jogosultak a CDN peremhálózati gyorsítótár. Ha módosít egy objektumot, amely jelenleg tárolja a hello CDN, hello új tartalmak nem lesznek elérhetők keresztül hello CDN amíg nem hello CDN tartalmát frissíti, hello gyorsítótárazott tartalom idő a működés közbeni időszak lejártával.
> 
> 

## <a name="step-5-remove-content-from-hello-cdn"></a>5. lépés: Tartalom eltávolítása a CDN hello
Ha már nem kívánja toocache egy objektumot a hello Azure Content Delivery Network (CDN), akkor hello lépések valamelyikét hajthatja végre:

* Hogy hello tároló privát nyilvános helyett. Lásd: [kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](../storage/blobs/storage-manage-access-to-resources.md) további információt.
* Tiltsa le, vagy törölni hello CDN-végpontot hello felügyeleti portál használatával.
* Az üzemeltetett szolgáltatás toono hosszabb válaszoljon toorequests hello objektum módosíthatja.

Az objektum már gyorsítótárazza a hello CDN gyorsítótárában marad, amíg hello objektum hello idő a működés közbeni időszak lejár, vagy hello végpont véglegesen törlődnek. Hello idő a működés közbeni időszak lejár, hello CDN ellenőrzi toosee e hello CDN-végpont továbbra is érvényes, és hello objektum névtelenül továbbra is elérhetők maradnak. Ha nem, majd hello objektum rendszer már nem gyorsítótárazható.

## <a name="additional-resources"></a>További források
* [Hogyan tooMap CDN tartalom tooa egyéni tartományhoz](cdn-map-content-to-custom-domain.md)
* [Az egyéni tartomány HTTPS engedélyezése](cdn-custom-ssl.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png
[cdn-enable-navigation]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png
[cdn-storage-new-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png
[cdn-storage-cdn-configuration]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png 
