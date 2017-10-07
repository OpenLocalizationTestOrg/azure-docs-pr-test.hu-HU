---
title: "virtuális merevlemez aaaUpload fájl tooAzure DevTest Labs segítségével a Microsoft Azure Tártallózó |} Microsoft Docs"
description: "Töltse fel a Microsoft Azure Tártallózó használó virtuális merevlemez fájl toolab tárfiókot"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
ms.openlocfilehash: 686691e3676cea4b2d7cd8bf045bc43a792c667e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-vhd-file-toolabs-storage-account-using-microsoft-azure-storage-explorer"></a>Töltse fel a Microsoft Azure Tártallózó használó virtuális merevlemez fájl toolab tárfiókot

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

Az Azure DevTest Labs szolgáltatásban a VHD-fájlokat lehet használt toocreate egyéni képek, amelyek használt tooprovision virtuális gépek. Ez a cikk bemutatja, hogyan toouse [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooupload egy VHD-fájl tooa tesztkörnyezet tárfiókja. A VHD-fájl feltöltése után hello [további lépések szakaszt](#next-steps) felsorolja az egyes cikkeket, amelyek bemutatják, hogyan toocreate hello egy egyéni lemezkép feltöltése a VHD-fájlt. További információ a lemezek és a VHD-ken az Azure-ban: [lemezek és a virtuális merevlemezek a virtuális gépek](../virtual-machines/linux/about-disks-and-vhds.md)

## <a name="step-by-step-instructions"></a>Lépésenkénti utasítások

következő lépések bejárása le a virtuális merevlemez feltöltése fájl használatával tooDevTest Labs hello [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md).

1. [Töltse le és telepítse a Microsoft Azure Tártallózó hello legújabb verziójának hello](http://www.storageexplorer.com).

1. Hello Azure-portál használatával hello tesztkörnyezet tárfiókja nevére hello beolvasása:

    1. Jelentkezzen be toohello [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040).
    
    1. Válassza ki **további szolgáltatások**, majd válassza ki **DevTest Labs** hello listából.
    
    1. Labs hello listában jelölje ki hello kívánt labor.  
    
    1. Hello labor paneljén válassza **konfigurációs**. 
    
    1. A tesztkörnyezet hello **konfigurációs** panelen válassza **egyéni lemezképeket (VHD)**.
    
    1. A hello **egyéni lemezképek** panelen válasszon ki **+ Hozzáadás**. 
    
    1. A hello **egyéni lemezkép** panelen válassza **VHD**.
    
    1. A hello **VHD** panelen válassza **a PowerShell használatával virtuális merevlemez feltöltéséhez**.
    
        ![Töltse fel a virtuális merevlemez a PowerShell használatával][0]
    
    1. Hello **PowerShell-lel lemezkép feltöltése a** panelt jeleníti meg a hívás toohello **Add-AzureVhd** parancsmag. az első paraméter hello (*cél*) hello a tárfióknév hello labor hello a következő formátumban tartalmazza:
    
        https://<STORAGE-ACCOUNT-Name>.BLOB.Core.Windows.NET/uploads/... 

    1. Jegyezze fel a hello tárfiók neve, a későbbi lépésekben használatban van.
    
1. Csatlakozás tooan Tártallózó Azure-előfizetés fiókot.

    > [!TIP] 
    > 
    > A Tártallózó kapcsolati beállításokat támogatja. Ez a rész az Azure-előfizetéshez társított csatlakozó tooa tárfiók mutat. toosee hello egyéb Tártallózó által támogatott kapcsolódási beállítások, tekintse meg a toohello cikk [Ismerkedés a Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md).
 
    1. Nyissa meg a Tártallózót.
    
    1. A Tártallózó, válassza ki a **Azure Fiókbeállítások**. 
    
        ![Azure-fiók beállításai][1]
    
    1. hello bal oldali ablaktáblán hello Microsoft-fiókkal bejelentkezett azt jeleníti meg. tooconnect tooanother fiókját, válassza **vegyen fel egy fiókot**, és hajtsa végre a hello párbeszédpanelek toosign be legalább egy aktív Azure-előfizetéssel társított Microsoft-fiókkal.
    
        ![Fiók hozzáadása][2]
    
    1. Miután sikeresen bejelentkezett egy Microsoft-fiókkal, hello bal oldali ablaktáblán tölti fel hello fiókhoz társított Azure-előfizetések. Válassza ki az Azure-előfizetést, amelyhez szeretné, hogy toowork, és válassza hello **alkalmaz**. (Kiválasztásával **előfizetéseket** megjelenítése és elrejtése az összes kijelölt hello, vagy nincs hello felsorolt Azure-előfizetések.)
    
        ![Azure-előfizetések kiválasztása][3]
    
    1. hello bal oldali panelen kiválasztott hello Azure-előfizetéssel társított tárfiókokat hello jeleníti meg.
    
        ![Kiválasztott Azure-előfizetések][4]

1. Keresse meg a hello tesztkörnyezet tárfiókja:

    1. Hello Tártallózó bal oldali ablaktáblán keresse meg, és bontsa ki az Azure-előfizetés hello labor birtokló hello hello csomópontot.
    
    1. Hello előfizetési csomópontot, bontsa ki a **Tárfiókok**.

    1. Bontsa ki a hello labor tárolási fiók csomópont tooreveal csomópontok a **Blobtárolók**, **fájlmegosztások**, **várólisták**, és **táblák**.
    
    1. Bontsa ki a hello **Blobtárolók** csomópont.
    
    1. Válasszon hello feltöltések blob tároló toodisplay tartalmának hello jobb oldali ablaktáblán.
        
        ![Directory feltöltése][5]

1. A Tártallózóval hello VHD-fájl feltöltése:

    1. Hello Tártallózó jobb oldali ablaktáblában kell megjelennie a hello hello BLOB listáját **feltölt** hello tesztkörnyezet tárfiókja blob tároló. Válassza hello blob szerkesztő eszköztár **feltöltése** 
        
        ![Töltse fel gomb][6]
    
    1. A hello **feltöltése** legördülő menüben válassza **fájlok feltöltése...** .
    
    1. A hello **fájlok feltöltése** párbeszédpanelen jelölje be hello három ponttal.
        
        ![Fájl kiválasztása][8]  

    1. A hello **válassza fájlok tooupload** párbeszédpanelen Tallózás toohello VHD-fájl szükséges, válassza ki azt, és válassza **nyitott**.
    
    1. Ha a visszaadott toohello **fájlok feltöltése** párbeszédpanelen módosítsa **Blob-típusú** túl**oldalakra vonatkozó Blob**.
    
    1. Válassza a **Feltöltés** lehetőséget.

        ![Fájl kiválasztása][9]  
    
    1. a Tártallózó hello **tevékenységnapló** ablaktábla megjeleníti azokat a hello letöltés állapota (valamint hivatkozások toocancel hello feltöltés). a VHD-fájl feltöltése hello folyamat megnőhet hello hello VHD-fájl méretétől és a kapcsolat sebességétől függően. 

        ![Fájlfeltöltés állapot][10]  

## <a name="next-steps"></a>Következő lépések

- [Hozzon létre egy egyéni lemezképet egy VHD-fájlt hello Azure-portálon az Azure DevTest Labs szolgáltatásban](devtest-lab-create-template.md)
- [Egyéni lemezkép létrehozása a PowerShell használatával VHD-fájl az Azure DevTest Labs szolgáltatásban](devtest-lab-create-custom-image-from-vhd-using-powershell.md)

[0]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-image-using-psh.png
[1]: ./media/devtest-lab-upload-vhd-using-storage-explorer/settings-icon.png
[2]: ./media/devtest-lab-upload-vhd-using-storage-explorer/add-account-link.png
[3]: ./media/devtest-lab-upload-vhd-using-storage-explorer/subscriptions-list.png
[4]: ./media/devtest-lab-upload-vhd-using-storage-explorer/storage-accounts-list.png
[5]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-dir.png
[6]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-button.png
[7]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-files.png
[8]: ./media/devtest-lab-upload-vhd-using-storage-explorer/select-file.png
[9]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-file.png
[10]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-status.png
