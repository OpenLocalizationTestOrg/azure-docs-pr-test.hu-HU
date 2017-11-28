---
title: "Állapot konfigurációs Resource Manager-sablon aaaDesired |} Microsoft Docs"
description: "A célállapot-konfiguráció Azure-példákkal és hibaelhárítási Resource Manager-sablon meghatározása"
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
ms.openlocfilehash: fc47ac149b1179d9305797eaa8ed8a83c0958c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a><span data-ttu-id="1e2ea-103">Windows VMSS és célállapot-konfiguráció Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="1e2ea-103">Windows VMSS and Desired State Configuration with Azure Resource Manager templates</span></span>
<span data-ttu-id="1e2ea-104">Ez a cikk ismerteti a hello Resource Manager-sablon hello [célállapot-konfiguráció bővítmény kezelő](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1e2ea-104">This article describes hello Resource Manager template for hello [Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

## <a name="template-example-for-a-windows-vm"></a><span data-ttu-id="1e2ea-105">A Windows virtuális gépek sablon – példa</span><span class="sxs-lookup"><span data-stu-id="1e2ea-105">Template example for a Windows VM</span></span>
<span data-ttu-id="1e2ea-106">hello következő kódrészlettel hiányzóra hello hello sablon erőforrás szakasza.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-106">hello following snippet goes into hello Resource section of hello template.</span></span>

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }

```

## <a name="template-example-for-windows-vmss"></a><span data-ttu-id="1e2ea-107">A Windows VMSS sablon – példa</span><span class="sxs-lookup"><span data-stu-id="1e2ea-107">Template example for Windows VMSS</span></span>
<span data-ttu-id="1e2ea-108">Egy VMSS fürtcsomópont hello "VirtualMachineProfile", "extensionProfile" attribútum a "Tulajdonságok" szakasz.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-108">A VMSS node has a "properties" section with hello "VirtualMachineProfile", "extensionProfile" attribute.</span></span> <span data-ttu-id="1e2ea-109">A DSC-ből "kiterjesztésekkel" alatt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-109">DSC is added under "extensions".</span></span> 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
        }
```

## <a name="detailed-settings-information"></a><span data-ttu-id="1e2ea-110">Részletes beállítási információk</span><span class="sxs-lookup"><span data-stu-id="1e2ea-110">Detailed Settings Information</span></span>
<span data-ttu-id="1e2ea-111">hello következő séma egy hello beállítási részén hello Azure Resource Manager-sablon Azure DSC-bővítményt.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-111">hello following schema is for hello settings portion of hello Azure DSC extension in an Azure Resource Manager template.</span></span>

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a><span data-ttu-id="1e2ea-112">Részletek</span><span class="sxs-lookup"><span data-stu-id="1e2ea-112">Details</span></span>
| <span data-ttu-id="1e2ea-113">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="1e2ea-113">Property Name</span></span> | <span data-ttu-id="1e2ea-114">Típus</span><span class="sxs-lookup"><span data-stu-id="1e2ea-114">Type</span></span> | <span data-ttu-id="1e2ea-115">Leírás</span><span class="sxs-lookup"><span data-stu-id="1e2ea-115">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1e2ea-116">settings.wmfVersion</span><span class="sxs-lookup"><span data-stu-id="1e2ea-116">settings.wmfVersion</span></span> |<span data-ttu-id="1e2ea-117">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1e2ea-117">string</span></span> |<span data-ttu-id="1e2ea-118">Megadja, hogy a virtuális Gépre telepíteni kell a Windows Management Framework hello hello verziószámát.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-118">Specifies hello version of hello Windows Management Framework that should be installed on your VM.</span></span> <span data-ttu-id="1e2ea-119">Ez a tulajdonság too'latest beállítása "telepíti hello a frissített verzió WMF.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-119">Setting this property too'latest' installs hello most updated version of WMF.</span></span> <span data-ttu-id="1e2ea-120">Ez a tulajdonság a lehetséges értékek csak az aktuális hello **"4.0", "5.0", "5.0PP', és a"legutóbbi"**.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-120">hello only current possible values for this property are **'4.0', '5.0', '5.0PP', and 'latest'**.</span></span> <span data-ttu-id="1e2ea-121">A lehetséges értékek: tulajdonos tooupdates.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-121">These possible values are subject tooupdates.</span></span> <span data-ttu-id="1e2ea-122">hello alapértelmezett érték: "legújabb".</span><span class="sxs-lookup"><span data-stu-id="1e2ea-122">hello default value is 'latest'.</span></span> |
| <span data-ttu-id="1e2ea-123">Settings.Configuration.URL</span><span class="sxs-lookup"><span data-stu-id="1e2ea-123">settings.configuration.url</span></span> |<span data-ttu-id="1e2ea-124">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1e2ea-124">string</span></span> |<span data-ttu-id="1e2ea-125">A DSC konfigurációs zip fájlt adja meg a hello URL-cím hely, mely toodownload.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-125">Specifies hello URL location from which toodownload your DSC configuration zip file.</span></span> <span data-ttu-id="1e2ea-126">Ha hello URL-cím egy SAS-jogkivonat hozzáférést igényel, meg kell tooset toohello hello protectedSettings.configurationUrlSasToken tulajdonság értékét a SAS-jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-126">If hello URL provided requires a SAS token for access, you need tooset hello protectedSettings.configurationUrlSasToken property toohello value of your SAS token.</span></span> <span data-ttu-id="1e2ea-127">E tulajdonság megadása kötelező, ha settings.configuration.script és/vagy settings.configuration.function vannak meghatározva.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-127">This property is required if settings.configuration.script and/or settings.configuration.function are defined.</span></span> |
| <span data-ttu-id="1e2ea-128">Settings.Configuration.Script</span><span class="sxs-lookup"><span data-stu-id="1e2ea-128">settings.configuration.script</span></span> |<span data-ttu-id="1e2ea-129">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1e2ea-129">string</span></span> |<span data-ttu-id="1e2ea-130">A DSC-konfiguráció hello definícióját tartalmazó parancsprogram hello hello fájl nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-130">Specifies hello file name of hello script that contains hello definition of your DSC configuration.</span></span> <span data-ttu-id="1e2ea-131">Ezt a parancsfájlt a hello hello configuration.url tulajdonság által megadott URL-CÍMRŐL letöltött zip-fájl hello hello gyökérmappájában kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-131">This script must be in hello root folder of hello zip file downloaded from hello URL specified by hello configuration.url property.</span></span> <span data-ttu-id="1e2ea-132">E tulajdonság megadása kötelező, ha settings.configuration.url és/vagy settings.configuration.script vannak meghatározva.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-132">This property is required if settings.configuration.url and/or settings.configuration.script are defined.</span></span> |
| <span data-ttu-id="1e2ea-133">Settings.Configuration.Function</span><span class="sxs-lookup"><span data-stu-id="1e2ea-133">settings.configuration.function</span></span> |<span data-ttu-id="1e2ea-134">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1e2ea-134">string</span></span> |<span data-ttu-id="1e2ea-135">A DSC-konfiguráció hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-135">Specifies hello name of your DSC configuration.</span></span> <span data-ttu-id="1e2ea-136">hello-konfiguráció configuration.script által meghatározott hello parancsfájl kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-136">hello configuration named must be contained in hello script defined by configuration.script.</span></span> <span data-ttu-id="1e2ea-137">E tulajdonság megadása kötelező, ha settings.configuration.url és/vagy settings.configuration.function vannak meghatározva.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-137">This property is required if settings.configuration.url and/or settings.configuration.function are defined.</span></span> |
| <span data-ttu-id="1e2ea-138">settings.configurationArguments</span><span class="sxs-lookup"><span data-stu-id="1e2ea-138">settings.configurationArguments</span></span> |<span data-ttu-id="1e2ea-139">Gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="1e2ea-139">Collection</span></span> |<span data-ttu-id="1e2ea-140">Határozza meg a paramétereket, milyen toopass tooyour DSC-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-140">Defines any parameters you would like toopass tooyour DSC configuration.</span></span> <span data-ttu-id="1e2ea-141">Ez a tulajdonság nincs titkosítva.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-141">This property is not encrypted.</span></span> |
| <span data-ttu-id="1e2ea-142">settings.configurationData.url</span><span class="sxs-lookup"><span data-stu-id="1e2ea-142">settings.configurationData.url</span></span> |<span data-ttu-id="1e2ea-143">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1e2ea-143">string</span></span> |<span data-ttu-id="1e2ea-144">Meghatározza a hello URL-címet, a mely toodownload a konfigurációs adatok (.psd1) toouse fájlt a DSC-konfiguráció bemenetként.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-144">Specifies hello URL from which toodownload your configuration data (.psd1) file toouse as input for your DSC configuration.</span></span> <span data-ttu-id="1e2ea-145">Ha hello URL-cím egy SAS-jogkivonat hozzáférést igényel, meg kell tooset toohello hello protectedSettings.configurationDataUrlSasToken tulajdonság értékét a SAS-jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-145">If hello URL provided requires a SAS token for access, you need tooset hello protectedSettings.configurationDataUrlSasToken property toohello value of your SAS token.</span></span> |
| <span data-ttu-id="1e2ea-146">settings.privacy.dataEnabled</span><span class="sxs-lookup"><span data-stu-id="1e2ea-146">settings.privacy.dataEnabled</span></span> |<span data-ttu-id="1e2ea-147">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1e2ea-147">string</span></span> |<span data-ttu-id="1e2ea-148">Engedélyezheti vagy letilthatja a telemetriai adatok gyűjtése.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-148">Enables or disables telemetry collection.</span></span> <span data-ttu-id="1e2ea-149">hello csak lehetséges Ez a tulajdonság értékei **"Engedélyezés", "Letiltás", ", vagy $null**.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-149">hello only possible values for this property are **'Enable', 'Disable', '', or $null**.</span></span> <span data-ttu-id="1e2ea-150">Így ez a tulajdonság üres vagy null értékű lehetővé teszi, hogy a telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-150">Leaving this property blank or null enables telemetry.</span></span> <span data-ttu-id="1e2ea-151">hello alapértelmezett érték ".</span><span class="sxs-lookup"><span data-stu-id="1e2ea-151">hello default value is ''.</span></span> [<span data-ttu-id="1e2ea-152">További információ</span><span class="sxs-lookup"><span data-stu-id="1e2ea-152">More Information</span></span>](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| <span data-ttu-id="1e2ea-153">settings.advancedOptions.downloadMappings</span><span class="sxs-lookup"><span data-stu-id="1e2ea-153">settings.advancedOptions.downloadMappings</span></span> |<span data-ttu-id="1e2ea-154">Gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="1e2ea-154">Collection</span></span> |<span data-ttu-id="1e2ea-155">Meghatározza, mely toodownload a WMF hello más helyekre.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-155">Defines alternate locations from which toodownload hello WMF.</span></span> [<span data-ttu-id="1e2ea-156">További információ</span><span class="sxs-lookup"><span data-stu-id="1e2ea-156">More Information</span></span>](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| <span data-ttu-id="1e2ea-157">protectedSettings.configurationArguments</span><span class="sxs-lookup"><span data-stu-id="1e2ea-157">protectedSettings.configurationArguments</span></span> |<span data-ttu-id="1e2ea-158">Gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="1e2ea-158">Collection</span></span> |<span data-ttu-id="1e2ea-159">Határozza meg a paramétereket, milyen toopass tooyour DSC-konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-159">Defines any parameters you would like toopass tooyour DSC configuration.</span></span> <span data-ttu-id="1e2ea-160">Ez a tulajdonság titkosítva van.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-160">This property is encrypted.</span></span> |
| <span data-ttu-id="1e2ea-161">protectedSettings.configurationUrlSasToken</span><span class="sxs-lookup"><span data-stu-id="1e2ea-161">protectedSettings.configurationUrlSasToken</span></span> |<span data-ttu-id="1e2ea-162">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1e2ea-162">string</span></span> |<span data-ttu-id="1e2ea-163">Meghatározza hello SAS-token tooaccess hello URL-címet configuration.url határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-163">Specifies hello SAS token tooaccess hello URL defined by configuration.url.</span></span> <span data-ttu-id="1e2ea-164">Ez a tulajdonság titkosítva van.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-164">This property is encrypted.</span></span> |
| <span data-ttu-id="1e2ea-165">protectedSettings.configurationDataUrlSasToken</span><span class="sxs-lookup"><span data-stu-id="1e2ea-165">protectedSettings.configurationDataUrlSasToken</span></span> |<span data-ttu-id="1e2ea-166">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="1e2ea-166">string</span></span> |<span data-ttu-id="1e2ea-167">Meghatározza hello SAS-token tooaccess hello URL-címet configurationData.url határozzák meg.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-167">Specifies hello SAS token tooaccess hello URL defined by configurationData.url.</span></span> <span data-ttu-id="1e2ea-168">Ez a tulajdonság titkosítva van.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-168">This property is encrypted.</span></span> |

## <a name="settings-vs-protectedsettings"></a><span data-ttu-id="1e2ea-169">Vs beállításait. ProtectedSettings</span><span class="sxs-lookup"><span data-stu-id="1e2ea-169">Settings vs. ProtectedSettings</span></span>
<span data-ttu-id="1e2ea-170">Minden beállítás beállítások szövegfájlba hello VM lesznek mentve.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-170">All settings are saved in a settings text file on hello VM.</span></span>
<span data-ttu-id="1e2ea-171">A "beállítások" olyan nyilvános tulajdonságok mert hello beállításait szöveges fájlban nincsenek titkosítva.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-171">Properties under 'settings' are public properties because they are not encrypted in hello settings text file.</span></span>
<span data-ttu-id="1e2ea-172">A "protectedSettings" tulajdonságok a tanúsítvánnyal titkosított, és csak akkor jelennek meg ebben a fájlban, a virtuális gép hello egyszerű szöveg.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-172">Properties under 'protectedSettings' are encrypted with a certificate and are not shown in plain text in this file on hello VM.</span></span>

<span data-ttu-id="1e2ea-173">Ha hello konfigurációs hitelesítő adatokat igényel, protectedSettings is szerepelni:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-173">If hello configuration needs credentials, they can be included in protectedSettings:</span></span>

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
               "userName": "UsernameValue1",
               "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a><span data-ttu-id="1e2ea-174">Példa</span><span class="sxs-lookup"><span data-stu-id="1e2ea-174">Example</span></span>
<span data-ttu-id="1e2ea-175">hello alábbi példa származik hello "Első lépések" szakasza hello [DSC kiterjesztés kezelője – áttekintés oldalra](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1e2ea-175">hello following example derives from hello "Getting Started" section of hello [DSC Extension Handler Overview page](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
<span data-ttu-id="1e2ea-176">A példa Resource Manager-sablonok parancsmagok toodeploy hello kiterjesztés helyett.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-176">This example uses Resource Manager templates instead of cmdlets toodeploy hello extension.</span></span> <span data-ttu-id="1e2ea-177">Hello "IisInstall.ps1" konfigurációs elmentéséhez elhelyezéséhez egy. A ZIP-fájl, és fájlfeltöltési hello az elérhető URL-címre.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-177">Save hello "IisInstall.ps1" configuration, place it in a .ZIP file, and upload hello file in an accessible URL.</span></span> <span data-ttu-id="1e2ea-178">A példában az Azure blob Storage tárolóban, de lehetséges toodownload. A ZIP-fájlok bármilyen tetszőleges helyről.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-178">This example uses Azure blob storage, but it is possible toodownload .ZIP files from any arbitrary location.</span></span>

<span data-ttu-id="1e2ea-179">A hello Azure Resource Manager sablon hello alábbira arra utasítja a hello VM toodownload hello megfelelő fájlt, és futtassa hello megfelelő PowerShell funkció:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-179">In hello Azure Resource Manager template, hello following code instructs hello VM toodownload hello correct file and run hello appropriate PowerShell function:</span></span>

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-hello-previous-format"></a><span data-ttu-id="1e2ea-180">Az előző formátum hello frissítése</span><span class="sxs-lookup"><span data-stu-id="1e2ea-180">Updating from hello Previous Format</span></span>
<span data-ttu-id="1e2ea-181">Hello előző formátum (nyilvános tulajdonságokat hello ModulesUrl, ConfigurationFunction, SasToken vagy tulajdonságait tartalmazó) automatikusan beállításait toohello aktuális formátumot támogató és csak megfelelően előtt.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-181">Any settings in hello previous format (containing hello public properties ModulesUrl, ConfigurationFunction, SasToken, or Properties) automatically adapt toohello current format and run just as they did before.</span></span>

<span data-ttu-id="1e2ea-182">a következő séma hello milyen hello előző beállítási séma hasonlóan keresni:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-182">hello following schema is what hello previous settings schema looked like:</span></span>

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points tooprivate Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

<span data-ttu-id="1e2ea-183">Ez hogyan hello előző formátum alkalmazkodik toohello aktuális formátuma:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-183">Here's how hello previous format adapts toohello current format:</span></span>

| <span data-ttu-id="1e2ea-184">Tulajdonság neve</span><span class="sxs-lookup"><span data-stu-id="1e2ea-184">Property Name</span></span> | <span data-ttu-id="1e2ea-185">Előző séma megfelelője</span><span class="sxs-lookup"><span data-stu-id="1e2ea-185">Previous Schema Equivalent</span></span> |
| --- | --- |
| <span data-ttu-id="1e2ea-186">settings.wmfVersion</span><span class="sxs-lookup"><span data-stu-id="1e2ea-186">settings.wmfVersion</span></span> |<span data-ttu-id="1e2ea-187">a beállítások. WMFVersion</span><span class="sxs-lookup"><span data-stu-id="1e2ea-187">settings.WMFVersion</span></span> |
| <span data-ttu-id="1e2ea-188">Settings.Configuration.URL</span><span class="sxs-lookup"><span data-stu-id="1e2ea-188">settings.configuration.url</span></span> |<span data-ttu-id="1e2ea-189">a beállítások. ModulesUrl</span><span class="sxs-lookup"><span data-stu-id="1e2ea-189">settings.ModulesUrl</span></span> |
| <span data-ttu-id="1e2ea-190">Settings.Configuration.Script</span><span class="sxs-lookup"><span data-stu-id="1e2ea-190">settings.configuration.script</span></span> |<span data-ttu-id="1e2ea-191">Beállítások első része. ConfigurationFunction (előtt "\\\\")</span><span class="sxs-lookup"><span data-stu-id="1e2ea-191">First part of settings.ConfigurationFunction (before '\\\\')</span></span> |
| <span data-ttu-id="1e2ea-192">Settings.Configuration.Function</span><span class="sxs-lookup"><span data-stu-id="1e2ea-192">settings.configuration.function</span></span> |<span data-ttu-id="1e2ea-193">Második része a beállításokat. ConfigurationFunction (után "\\\\")</span><span class="sxs-lookup"><span data-stu-id="1e2ea-193">Second part of settings.ConfigurationFunction (after '\\\\')</span></span> |
| <span data-ttu-id="1e2ea-194">settings.configurationArguments</span><span class="sxs-lookup"><span data-stu-id="1e2ea-194">settings.configurationArguments</span></span> |<span data-ttu-id="1e2ea-195">a beállítások. Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="1e2ea-195">settings.Properties</span></span> |
| <span data-ttu-id="1e2ea-196">settings.configurationData.url</span><span class="sxs-lookup"><span data-stu-id="1e2ea-196">settings.configurationData.url</span></span> |<span data-ttu-id="1e2ea-197">protectedSettings.DataBlobUri (nélkül SAS-jogkivonat)</span><span class="sxs-lookup"><span data-stu-id="1e2ea-197">protectedSettings.DataBlobUri (without SAS token)</span></span> |
| <span data-ttu-id="1e2ea-198">settings.privacy.dataEnabled</span><span class="sxs-lookup"><span data-stu-id="1e2ea-198">settings.privacy.dataEnabled</span></span> |<span data-ttu-id="1e2ea-199">a beállítások. Privacy.DataEnabled</span><span class="sxs-lookup"><span data-stu-id="1e2ea-199">settings.Privacy.DataEnabled</span></span> |
| <span data-ttu-id="1e2ea-200">settings.advancedOptions.downloadMappings</span><span class="sxs-lookup"><span data-stu-id="1e2ea-200">settings.advancedOptions.downloadMappings</span></span> |<span data-ttu-id="1e2ea-201">a beállítások. AdvancedOptions.DownloadMappings</span><span class="sxs-lookup"><span data-stu-id="1e2ea-201">settings.AdvancedOptions.DownloadMappings</span></span> |
| <span data-ttu-id="1e2ea-202">protectedSettings.configurationArguments</span><span class="sxs-lookup"><span data-stu-id="1e2ea-202">protectedSettings.configurationArguments</span></span> |<span data-ttu-id="1e2ea-203">protectedSettings.Properties</span><span class="sxs-lookup"><span data-stu-id="1e2ea-203">protectedSettings.Properties</span></span> |
| <span data-ttu-id="1e2ea-204">protectedSettings.configurationUrlSasToken</span><span class="sxs-lookup"><span data-stu-id="1e2ea-204">protectedSettings.configurationUrlSasToken</span></span> |<span data-ttu-id="1e2ea-205">a beállítások. SasToken</span><span class="sxs-lookup"><span data-stu-id="1e2ea-205">settings.SasToken</span></span> |
| <span data-ttu-id="1e2ea-206">protectedSettings.configurationDataUrlSasToken</span><span class="sxs-lookup"><span data-stu-id="1e2ea-206">protectedSettings.configurationDataUrlSasToken</span></span> |<span data-ttu-id="1e2ea-207">A protectedSettings.DataBlobUri SAS-jogkivonat</span><span class="sxs-lookup"><span data-stu-id="1e2ea-207">SAS token from protectedSettings.DataBlobUri</span></span> |

## <a name="troubleshooting---error-code-1100"></a><span data-ttu-id="1e2ea-208">Hibaelhárítás – 1100-as hibakód</span><span class="sxs-lookup"><span data-stu-id="1e2ea-208">Troubleshooting - Error Code 1100</span></span>
<span data-ttu-id="1e2ea-209">1100-as hibakód azt jelzi, hogy felhasználói bevitel toohello DSC-bővítmény hello probléma van.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-209">Error Code 1100 indicates that there is a problem with hello user input toohello DSC extension.</span></span>
<span data-ttu-id="1e2ea-210">Ezek a hibák szövegét hello változó, és módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-210">hello text of these errors is variable and may change.</span></span>
<span data-ttu-id="1e2ea-211">Az alábbiakban néhány hello hibákat tapasztal előfordulhat, hogy és az hogyan oldható meg őket.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-211">Here are some of hello errors you may run into and how you can fix them.</span></span>

### <a name="invalid-values"></a><span data-ttu-id="1e2ea-212">Érvénytelen értékekkel</span><span class="sxs-lookup"><span data-stu-id="1e2ea-212">Invalid Values</span></span>
<span data-ttu-id="1e2ea-213">"Privacy.dataCollection nem"{0}".</span><span class="sxs-lookup"><span data-stu-id="1e2ea-213">"Privacy.dataCollection is '{0}'.</span></span> <span data-ttu-id="1e2ea-214">hello csak a lehetséges értékek: ","Engedélyezése"és"Disable"" "WmfVersion nem"{0}".</span><span class="sxs-lookup"><span data-stu-id="1e2ea-214">hello only possible values are '', 'Enable', and 'Disable'" "WmfVersion is '{0}'.</span></span> <span data-ttu-id="1e2ea-215">Csak a lehetséges értékek a következők...</span><span class="sxs-lookup"><span data-stu-id="1e2ea-215">Only possible values are …</span></span> <span data-ttu-id="1e2ea-216">és a "legújabb" "</span><span class="sxs-lookup"><span data-stu-id="1e2ea-216">and 'latest'"</span></span>

<span data-ttu-id="1e2ea-217">Probléma: A megadott érték nem megengedett.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-217">Problem: A provided value is not allowed.</span></span>

<span data-ttu-id="1e2ea-218">Megoldás: Hello érvénytelen érték tooa érvényes értékének módosítása.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-218">Solution: Change hello invalid value tooa valid value.</span></span> <span data-ttu-id="1e2ea-219">Hello táblázatban hello Részletek területen találja.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-219">See hello table in hello Details section.</span></span>

### <a name="invalid-url"></a><span data-ttu-id="1e2ea-220">Érvénytelen URL-címe</span><span class="sxs-lookup"><span data-stu-id="1e2ea-220">Invalid URL</span></span>
<span data-ttu-id="1e2ea-221">"ConfigurationData.url nem"{0}".</span><span class="sxs-lookup"><span data-stu-id="1e2ea-221">"ConfigurationData.url is '{0}'.</span></span> <span data-ttu-id="1e2ea-222">Ez nem egy érvényes URL-címet az""DataBlobUri nem "{0}".</span><span class="sxs-lookup"><span data-stu-id="1e2ea-222">This is not a valid URL" "DataBlobUri is '{0}'.</span></span> <span data-ttu-id="1e2ea-223">Ez nem egy érvényes URL-címet az""Configuration.url nem "{0}".</span><span class="sxs-lookup"><span data-stu-id="1e2ea-223">This is not a valid URL" "Configuration.url is '{0}'.</span></span> <span data-ttu-id="1e2ea-224">Ez nem egy érvényes URL-címet az"</span><span class="sxs-lookup"><span data-stu-id="1e2ea-224">This is not a valid URL"</span></span>

<span data-ttu-id="1e2ea-225">Probléma: A megadott URL-cím érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-225">Problem: A provided URL is not valid.</span></span>

<span data-ttu-id="1e2ea-226">Megoldás: Ellenőrizze az a megadott URL-címet.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-226">Solution: Check all your provided URLs.</span></span> <span data-ttu-id="1e2ea-227">Győződjön meg arról, hogy az URL-címet megoldásához toovalid helyek hozzáférő hello bővítmény hello távoli számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-227">Make sure all URLs resolve toovalid locations that hello extension can access on hello remote machine.</span></span>

### <a name="invalid-configurationargument-type"></a><span data-ttu-id="1e2ea-228">Érvénytelen ConfigurationArgument típusa</span><span class="sxs-lookup"><span data-stu-id="1e2ea-228">Invalid ConfigurationArgument Type</span></span>
<span data-ttu-id="1e2ea-229">"Típus érvénytelen configurationArguments" {0}</span><span class="sxs-lookup"><span data-stu-id="1e2ea-229">"Invalid configurationArguments type {0}"</span></span>

<span data-ttu-id="1e2ea-230">Probléma: hello ConfigurationArguments tulajdonság nem oldható fel tooa Hashtable objektum.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-230">Problem: hello ConfigurationArguments property cannot resolve tooa Hashtable object.</span></span> 

<span data-ttu-id="1e2ea-231">Megoldás: Ellenőrizze a ConfigurationArguments tulajdonság egy kivonattáblát.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-231">Solution: Make your ConfigurationArguments property a Hashtable.</span></span> <span data-ttu-id="1e2ea-232">Hello portáladatbázis előző példában megadott hello formátumot követi.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-232">Follow hello format provided in hello preceeding example.</span></span> <span data-ttu-id="1e2ea-233">Figyeljen az ajánlatok, vesszővel válassza el egymástól, és kell használni.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-233">Watch out for quotes, commas, and braces.</span></span>

### <a name="duplicate-configurationarguments"></a><span data-ttu-id="1e2ea-234">Ismétlődő ConfigurationArguments</span><span class="sxs-lookup"><span data-stu-id="1e2ea-234">Duplicate ConfigurationArguments</span></span>
<span data-ttu-id="1e2ea-235">"Található nyilvános és a védett configurationArguments ismétlődő argumentumok"{0}""</span><span class="sxs-lookup"><span data-stu-id="1e2ea-235">"Found duplicate arguments '{0}' in both public and protected configurationArguments"</span></span>

<span data-ttu-id="1e2ea-236">Probléma: hello ConfigurationArguments nyilvános beállításai és hello védett beállításai ConfigurationArguments tartalmaz hello értékkel rendelkező tulajdonságok ugyanazzal a névvel.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-236">Problem: hello ConfigurationArguments in public settings and hello ConfigurationArguments in protected settings contain properties with hello same name.</span></span>

<span data-ttu-id="1e2ea-237">Megoldás: Távolítsa el az ismétlődő tulajdonságok hello egyik.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-237">Solution: Remove one of hello duplicate properties.</span></span>

### <a name="missing-properties"></a><span data-ttu-id="1e2ea-238">Hiányzó tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="1e2ea-238">Missing Properties</span></span>
<span data-ttu-id="1e2ea-239">"Megköveteli, hogy configuration.url configuration.module meg van adva vagy Configuration.function"</span><span class="sxs-lookup"><span data-stu-id="1e2ea-239">"Configuration.function requires that configuration.url or configuration.module is specified"</span></span>

<span data-ttu-id="1e2ea-240">"Megköveteli, hogy configuration.script megadott Configuration.url"</span><span class="sxs-lookup"><span data-stu-id="1e2ea-240">"Configuration.url requires that configuration.script is specified"</span></span>

<span data-ttu-id="1e2ea-241">"Megköveteli, hogy configuration.url megadott Configuration.script"</span><span class="sxs-lookup"><span data-stu-id="1e2ea-241">"Configuration.script requires that configuration.url is specified"</span></span>

<span data-ttu-id="1e2ea-242">"Megköveteli, hogy configuration.function megadott Configuration.url"</span><span class="sxs-lookup"><span data-stu-id="1e2ea-242">"Configuration.url requires that configuration.function is specified"</span></span>

<span data-ttu-id="1e2ea-243">"Megköveteli, hogy configuration.url megadott ConfigurationUrlSasToken"</span><span class="sxs-lookup"><span data-stu-id="1e2ea-243">"ConfigurationUrlSasToken requires that configuration.url is specified"</span></span>

<span data-ttu-id="1e2ea-244">"Megköveteli, hogy configurationData.url megadott ConfigurationDataUrlSasToken"</span><span class="sxs-lookup"><span data-stu-id="1e2ea-244">"ConfigurationDataUrlSasToken requires that configurationData.url is specified"</span></span>

<span data-ttu-id="1e2ea-245">Probléma: Egy meghatározott tulajdonság kell, hogy hiányzik egy másik tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-245">Problem: A defined property needs another property that is missing.</span></span>

<span data-ttu-id="1e2ea-246">Megoldások:</span><span class="sxs-lookup"><span data-stu-id="1e2ea-246">Solutions:</span></span> 

* <span data-ttu-id="1e2ea-247">Adja meg a hiányzó hello tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-247">Provide hello missing property.</span></span>
* <span data-ttu-id="1e2ea-248">Távolítsa el, amelyet a hello hiányzó tulajdonság hello tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="1e2ea-248">Remove hello property that needs hello missing property.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e2ea-249">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1e2ea-249">Next Steps</span></span>
<span data-ttu-id="1e2ea-250">További tudnivalók a DSC-ből és a virtuálisgép-skálázási készletekben [használatával virtuálisgép-skálázási készletekben a hello Azure DSC-bővítményt](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)</span><span class="sxs-lookup"><span data-stu-id="1e2ea-250">Learn about DSC and virtual machine scale sets in [Using Virtual Machine Scale Sets with hello Azure DSC Extension](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)</span></span>

<span data-ttu-id="1e2ea-251">További részletekért található a következő [DSC biztonságos hitelesítőadat-kezelés](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1e2ea-251">Find more details on [DSC's secure credential management](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="1e2ea-252">Hello Azure DSC-bővítmény kezelő további információkért lásd: [bemutatása toohello Azure célállapot-konfiguráció bővítmény kezelő](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1e2ea-252">For more information on hello Azure DSC extension handler, see [Introduction toohello Azure Desired State Configuration extension handler](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

<span data-ttu-id="1e2ea-253">További információ a PowerShell DSC [látogasson el a hello PowerShell dokumentációs központban](https://msdn.microsoft.com/powershell/dsc/overview).</span><span class="sxs-lookup"><span data-stu-id="1e2ea-253">For more information about PowerShell DSC, [visit hello PowerShell documentation center](https://msdn.microsoft.com/powershell/dsc/overview).</span></span> 

