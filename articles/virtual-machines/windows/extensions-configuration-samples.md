---
title: "Windows Virtuálisgép-bővítmények aaaSample konfigurációja |} Microsoft Docs"
description: "Mintakonfiguráció kiterjesztésű sablonok készítése"
services: virtual-machines-windows
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 0a1cee6c-51ea-4c03-b607-f158586d7175
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: 7697be969dbcf609423f64b75c7edf80ca1bfd9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-windows-vm-extension-configuration-samples"></a><span data-ttu-id="244f3-103">Azure-beli windowsos VM-bővítmények konfigurációs mintái</span><span class="sxs-lookup"><span data-stu-id="244f3-103">Azure Windows VM Extension Configuration Samples</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="244f3-104">PowerShell - sablon</span><span class="sxs-lookup"><span data-stu-id="244f3-104">PowerShell - Template</span></span>](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
> * [<span data-ttu-id="244f3-105">CLI - sablon</span><span class="sxs-lookup"><span data-stu-id="244f3-105">CLI - Template</span></span>](../linux/extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<br>

<span data-ttu-id="244f3-106">Ez a cikk ismerteti a mintakonfiguráció konfigurálása az Azure virtuális gép bővítményei Windows virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="244f3-106">This article provides sample configuration for configuring Azure VM Extensions for Windows VMs.</span></span>

<span data-ttu-id="244f3-107">toolearn ezen bővítményei kapcsolatos további információkért lásd: [Azure Virtuálisgép-bővítmények áttekintése.](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="244f3-107">toolearn more about these extensions, see [Azure VM Extensions Overview.](extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

<span data-ttu-id="244f3-108">toolearn bővítmény sablonok készítése kapcsolatos további információkért lásd: [bővítmény sablonok készítése.](extensions-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="244f3-108">toolearn more about authoring extension templates, see [Authoring Extension Templates.](extensions-authoring-templates.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

<span data-ttu-id="244f3-109">Ez a cikk a Windows hello részénél elvárt konfiguráció értékeit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="244f3-109">This article lists expected configuration values for some of hello Windows Extensions.</span></span>

## <a name="sample-template-snippet-for-vm-extensions-with-iaas-vms"></a><span data-ttu-id="244f3-110">Minta sablon részlet a Virtuálisgép-bővítmények az infrastruktúra-szolgáltatási virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="244f3-110">Sample template snippet for VM Extensions with IaaS VMs.</span></span>
<span data-ttu-id="244f3-111">hello sablon részlet üzembe helyezéséhez bővítmények keresi a következőként:</span><span class="sxs-lookup"><span data-stu-id="244f3-111">hello template snippet for Deploying extensions looks as following:</span></span>

      {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "MyExtension",
      "apiVersion": "2015-05-01-preview",
      "location": "[parameters('location')]",
      "dependsOn": ["[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"],
      "properties":
      {
      "publisher": "Publisher Namespace",
      "type": "extension Name",
      "typeHandlerVersion": "extension version",
      "autoUpgradeMinorVersion":true,
      "settings": {
      // Extension specific configuration goes in here.
      }
      }
      }

## <a name="sample-template-snippet-for-vm-extensions-with-vm-scale-sets"></a><span data-ttu-id="244f3-112">Minta sablon részlet Virtuálisgép-bővítmények a Virtuálisgép-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="244f3-112">Sample template snippet for VM Extensions with VM Scale Sets.</span></span>
    {
     "type":"Microsoft.Compute/virtualMachineScaleSets",
    ....
           "extensionProfile":{
           "extensions":[
             {
               "name":"extension Name",
               "properties":{
                 "publisher":"Publisher Namespace",
                 "type":"extension Name",
                 "typeHandlerVersion":"extension version",
                 "autoUpgradeMinorVersion":true,
                 "settings":{
                 // Extension specific configuration goes in here.
                 }
               }
              }
            }
          }

<span data-ttu-id="244f3-113">Hello bővítmény telepítése előtt ellenőrizze a hello bővítmény legújabb, és cserélje le a hello "typeHandlerVersion" hello aktuális legújabb verziójával.</span><span class="sxs-lookup"><span data-stu-id="244f3-113">Before deploying hello extension please check hello latest extension version and replace hello "typeHandlerVersion" with hello current latest version.</span></span>

<span data-ttu-id="244f3-114">Hello cikk többi minta konfigurációi Windows Virtuálisgép-bővítmények biztosít.</span><span class="sxs-lookup"><span data-stu-id="244f3-114">Rest of hello article provides sample configurations for Windows VM Extensions.</span></span>

<span data-ttu-id="244f3-115">Hello bővítmény telepítése előtt ellenőrizze a hello bővítmény legújabb, és cserélje le a hello "typeHandlerVersion" hello aktuális legújabb verziójával.</span><span class="sxs-lookup"><span data-stu-id="244f3-115">Before deploying hello extension please check hello latest extension version and replace hello "typeHandlerVersion" with hello current latest version.</span></span>

### <a name="customscript-extension-14"></a><span data-ttu-id="244f3-116">CustomScript bővítménnyel 1.4.</span><span class="sxs-lookup"><span data-stu-id="244f3-116">CustomScript Extension 1.4.</span></span>
      {
          "publisher": "Microsoft.Compute",
          "type": "CustomScriptExtension",
          "typeHandlerVersion": "1.4",
          "settings": {
              "fileUris": [
                  "http: //Yourstorageaccount.blob.core.windows.net/customscriptfiles/start.ps1"
              ],
              "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -start.ps1"
          },
          "protectedSettings": {
            "storageAccountName": "yourStorageAccountName",
            "storageAccountKey": "yourStorageAccountKey"
          }
      }

#### <a name="parameter-description"></a><span data-ttu-id="244f3-117">A paraméter leírását:</span><span class="sxs-lookup"><span data-stu-id="244f3-117">Parameter description:</span></span>
* <span data-ttu-id="244f3-118">fileUris: hello VM hello bővítmény által letöltött fájlok hello URL-címe vesszővel elválasztva listáját.</span><span class="sxs-lookup"><span data-stu-id="244f3-118">fileUris : Comma seperated list of urls of hello files that will be downloaded on hello VM by hello Extension.</span></span> <span data-ttu-id="244f3-119">Nincsenek fájlok lesznek letöltve, ha semmi sincs megadva.</span><span class="sxs-lookup"><span data-stu-id="244f3-119">No files are downloaded if nothing is specified.</span></span> <span data-ttu-id="244f3-120">Ha hello fájlok az Azure Storage, hello fileURLs titkos jelölhető és hello correspoding storageAccountName és storageAccountKey argumentumként átadhatók titkos paraméterek tooaccess ezeket a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="244f3-120">If hello files are in Azure Storage, hello fileURLs can be marked private and hello correspoding storageAccountName and storageAccountKey can be passed as private parameters tooaccess these files.</span></span>
* <span data-ttu-id="244f3-121">commandToExecute: [kötelező paraméter]: Ez a bővítmény hello által végrehajtott parancs hello.</span><span class="sxs-lookup"><span data-stu-id="244f3-121">commandToExecute : [Mandatory Parameter] : This is hello command that will be executed by hello Extension.</span></span>
* <span data-ttu-id="244f3-122">storageAccountName: [nem kötelező paraméter]: hello fileURLs elérését, ha a titkos vannak beállítva a Tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="244f3-122">storageAccountName : [Optional Parameter] : Storage Account Name for accessing hello fileURLs, if they are marked as private.</span></span>
* <span data-ttu-id="244f3-123">storageAccountKey: [nem kötelező paraméter]: Tárfiók eléréséhez szükséges hello fileURLs, ha vannak beállítva a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="244f3-123">storageAccountKey : [Optional Parameter] : Storage Account Key for accessing hello fileURLs, if they are marked as private.</span></span>

### <a name="customscript-extension-17"></a><span data-ttu-id="244f3-124">CustomScript bővítménnyel 1.7.</span><span class="sxs-lookup"><span data-stu-id="244f3-124">CustomScript Extension 1.7.</span></span>
<span data-ttu-id="244f3-125">Tekintse meg az 1.4-es verziójának tooCustomScript paraméter leírását.</span><span class="sxs-lookup"><span data-stu-id="244f3-125">Please refer tooCustomScript version 1.4 for parameter description.</span></span> <span data-ttu-id="244f3-126">1.7-es verziójának bevezeti a parancsfájl parameters(commandToExecute) küldjük protectedSettings, ebben az esetben azok titkosítva legyen elküldése előtt.</span><span class="sxs-lookup"><span data-stu-id="244f3-126">Version 1.7 introduces support for sending script parameters(commandToExecute) as protectedSettings, in which case they will be encrypted before sending.</span></span> <span data-ttu-id="244f3-127">a beállítások protectedSettings, de nem mind a "commandToExecute" paraméter adható meg.</span><span class="sxs-lookup"><span data-stu-id="244f3-127">'commandToExecute' parameter can be specified either in settings or protectedSettings but not in both.</span></span>

        {
            "publisher": "Microsoft.Compute",
            "type": "CustomScriptExtension",
            "typeHandlerVersion": "1.7",
            "settings": {
                "fileUris": [
                    "http: //Yourstorageaccount.blob.core.windows.net/customscriptfiles/start.ps1"
                ],
                "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -start.ps1"
            },
            "protectedSettings": {
              "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -start.ps1",
              "storageAccountName": "yourStorageAccountName",
              "storageAccountKey": "yourStorageAccountKey"
            }
        }

### <a name="vmaccess-extension"></a><span data-ttu-id="244f3-128">VMAccess bővítmény.</span><span class="sxs-lookup"><span data-stu-id="244f3-128">VMAccess Extension.</span></span>
      {
          "publisher": "Microsoft.Compute",
          "type": "VMAccessAgent",
          "typeHandlerVersion": "2.0",
          "settings": {
            "UserName" : "New User Name"
          },
          "protectedSettings": {
            "Password" : "New Password"
          }
      }

### <a name="dsc-extension"></a><span data-ttu-id="244f3-129">A DSC-bővítményt.</span><span class="sxs-lookup"><span data-stu-id="244f3-129">DSC Extension.</span></span>
      {
          "publisher": "Microsoft.Powershell",
          "type": "DSC",
          "typeHandlerVersion": "2.1(Recommendation is toouse hello latest version)",
          "settings": {
              "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
              "SasToken": "Optional : SAS Token if ModulesUrl points tooAzure Blob Storage",
              "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
              "Properties": {
                  "ParameterToConfigurationFunction1": "Value1",
                  "ParameterToConfigurationFunction2": "Value2",
                  "ParameterOfTypePSCredential1": {
                      "UserName": "UsernameValue1",
                      "Password": "PrivateSettingsRef:Key1(Value is a reference tooa member of hello Items object in hello protected settings)"
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
              "DataBlobUri": "optional : https: //UrlToConfigurationData.psd1"
          }
      }


### <a name="symantec-endpoint-protection"></a><span data-ttu-id="244f3-130">Symantec Endpoint Protection.</span><span class="sxs-lookup"><span data-stu-id="244f3-130">Symantec Endpoint Protection.</span></span>
      {
        "publisher": "SymantecEndpointProtection",
        "type": "Symantec",
        "typeHandlerVersion": "12.1",
        "settings": {}
      }

### <a name="trend-micro-deep-security-agent"></a><span data-ttu-id="244f3-131">Trend Micro mély biztonsági ügynök.</span><span class="sxs-lookup"><span data-stu-id="244f3-131">Trend Micro Deep Security Agent.</span></span>
      {
        "publisher": "TrendMicro.DeepSecurity",
        "type": "TrendMicroDSA",
        "typeHandlerVersion": "9.6",
        "settings": {
          "ManagerAddress" : "Enter hello externally accessible DNS name or IP address of hello Deep Security Manager. Please enter \"agents.deepsecurity.trendmicro.com\" if using Deep Security as a Service",

          "ActivationPort" : "Enter hello port number of hello Deep Security Manager, default value - 443",

          "TenantIdentifier" : "Enter hello tenant ID, which is a hyphenated, 36-character string available in hello Deployment Scripts dialog box in hello Deep Security console. This parameter is mandatory if using Deep Security as a Service, or a multi-tenant installation of Deep Security Manager. Type NA if using a non multi-tenant installation of Deep Security Manager.",

          "TenantActivationPassword" : "Enter hello tenant activation password, which is a hyphenated, 36-character string available in hello Deployment Scripts dialog box in hello Deep Security console. This parameter is mandatory if using Deep Security as a Service, or a multi-tenant installation of Deep Security Manager. Type NA if using a non multi-tenant installation of Deep Security Manager.",

          "SecurityPolicy" : "Optional : Enter hello name or numeric ID of hello security policy defined in hello Deep Security Manager which will be applied on agent activation tooprotect this virtual machine (recommended). No security policy will be applied toohello virtual machine if this parameter is blank. This parameter is optional if using Deep Security as a Service."
        }
      }

### <a name="vormertric-transparent-encryption-agent"></a><span data-ttu-id="244f3-132">Vormertric átlátható titkosítási ügynök.</span><span class="sxs-lookup"><span data-stu-id="244f3-132">Vormertric Transparent Encryption Agent.</span></span>
            {
              "publisher": "Vormetric",
              "type": "VormetricTransparentEncryptionAgent",
              "typeHandlerVersion": "5.2",
              "settings": {
              }
            }

### <a name="puppet-enterprise-agent"></a><span data-ttu-id="244f3-133">Puppet Enterprise-ügynök.</span><span class="sxs-lookup"><span data-stu-id="244f3-133">Puppet Enterprise Agent.</span></span>
            {
              "publisher": "PuppetLabs",
              "type": "PuppetEnterpriseAgent",
              "typeHandlerVersion": "3.2",
              "settings": {
                "puppet_master_server" : "Puppet Master Server Name"
              }
            }  

### <a name="microsoft-monitoring-agent-for-azure-operational-insights"></a><span data-ttu-id="244f3-134">Microsoft-Figyelőügynök az Azure Operational Insights</span><span class="sxs-lookup"><span data-stu-id="244f3-134">Microsoft Monitoring Agent for Azure Operational Insights</span></span>
            {
              "publisher": "Microsoft.EnterpriseCloud.Monitoring",
              "type": "MicrosoftMonitoringAgent",
              "typeHandlerVersion": "1.0",
              "settings": {
                "workspaceId" : "hello Workspace ID is available from within hello Direct Agent Configuration section of hello Azure Operational Insights portal"
              }
              "protectedSettings": {
                "workspaceKey"  : "hello Workspace Key is a string that is available from within hello Direct Agent Configuration section of hello Azure Operational Insights portal"
              }
              }
            }

### <a name="mcafee-endpointsecurity"></a><span data-ttu-id="244f3-135">McAfee EndpointSecurity</span><span class="sxs-lookup"><span data-stu-id="244f3-135">McAfee EndpointSecurity</span></span>
            {
              "publisher": "McAfee.EndpointSecurity",
              "type": "McAfeeEndpointSecurity",
              "typeHandlerVersion": "6.0",
              "settings": {
                "entitlementKey" : "Optional : Enter a valid entitlement key or leave blank for trial version",
                "featureVS"      : "Choose whether or not tooinstall hello Virus and Spyware Protection features : true|false",
                "featureBP"      : "Choose whether or not tooinstall hello Browser Protection feature : true|false",
                "featureFW"      : "Choose whether or not tooinstall hello Firewall Protection feature :true|false",
                "relayServer"    : "Allows VMs on hello local subnet tooreceive updates through this VM when they are not connected toohello internet : true|false"
              }
            }

### <a name="azure-iaas-antimalware"></a><span data-ttu-id="244f3-136">Az Azure infrastruktúra-szolgáltatási kártevőirtó</span><span class="sxs-lookup"><span data-stu-id="244f3-136">Azure IaaS Antimalware</span></span>
          {
            "publisher": "Microsoft.Azure.Security",
            "type": "IaaSAntimalware",
            "typeHandlerVersion": "1.2",
            "settings": {
              "AntimalwareEnabled": "true",
              "ExclusionsPaths"        : "Optional : ExclusionsPaths",
              "ExclusionsExtensions"   : "Optional : ExclusionsExtensions",
              "ExclusionsProcesses"   : "Optional : ExclusionsProcesses",
              "RealtimeProtectionEnabled"   : "Optional : True|False",
              "ScheduledScanSettingsIsEnabled"   : "Optional : True|False",
              "ScheduledScanSettingsScanType"   : "Optional : Quick|Full",
              "ScheduledScanSettingsDay"   : "Optional : Sunday-Saturday",
              "ScheduledScanSettingsTime"   : "Optional : When tooperform hello scheduled scan, measured in minutes from midnight,0-1440"
            }
          }

### <a name="eset-file-security"></a><span data-ttu-id="244f3-137">ESET fájl biztonsági</span><span class="sxs-lookup"><span data-stu-id="244f3-137">ESET File Security</span></span>
          {
            "publisher": "ESET",
            "type": "FileSecurity",
            "typeHandlerVersion": "6.0",
            "settings": {
            }
          }

### <a name="datadog-agent"></a><span data-ttu-id="244f3-138">Datadog ügynök</span><span class="sxs-lookup"><span data-stu-id="244f3-138">Datadog Agent</span></span>
          {
            "publisher": "Datadog.Agent",
            "type": "DatadogWindowsAgent",
            "typeHandlerVersion": "0.5",
            "settings": {
              "api_key" : "API Key from https://app.datadoghq.com/account/settings#api"
            }
          }

### <a name="confer-advanced-threat-prevention-and-incident-response-for-azure"></a><span data-ttu-id="244f3-139">Az Advanced Threat megelőzése és Incidensekre adott reakciók révén az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="244f3-139">Confer Advanced Threat Prevention and Incident Response for Azure</span></span>
          {
            "publisher": "Confer",
            "type": "ConferForAzure",
            "typeHandlerVersion": "1.0",
            "settings": {
              "ConferRegisterCode" : "Optional : Valid product registration code or leave it blank tooregister later",
              "ConferRegisterCode" : "Enter a valid server name if your account requires a dedicated confer backend server or leave it blank"
            }
          }

### <a name="cloudlink-securevm-agent"></a><span data-ttu-id="244f3-140">CloudLink SecureVM ügynök</span><span class="sxs-lookup"><span data-stu-id="244f3-140">CloudLink SecureVM Agent</span></span>
          {
            "publisher": "CloudLinkEMC.SecureVM",
            "type": "CloudLinkSecureVMWindowsAgent",
            "typeHandlerVersion": "4.0",
            "settings": {
              "CloudLinkCenter" : "specify valid IP/FQDN tooCloudLinkCenter"
            }
          }

### <a name="barracuda-vpn-connectivity-agent-for-microsoft-azure"></a><span data-ttu-id="244f3-141">Barracuda VPN kapcsolatot ügynök a Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="244f3-141">Barracuda VPN Connectivity Agent for Microsoft Azure</span></span>
          {
            "publisher": "Barracuda.Azure.ConnectivityAgent",
            "type": "BarracudaConnectivityAgent",
            "typeHandlerVersion": "3.5",
            "settings": {
              "ServerAddress" : "Host name or IP address of hello VPN server - AES, AES256, Blowfish,CAST,DES,3DES,None",
              "EncryptionAlgorithm" : "Algorithm used tooencrypt VPN traffic - MD5,SHA1,SHA256,None",
              "PKCS12File" : "Url for file containing certificate and private key used tooauthenticate against hello VPN server",
              "PKCS12FilePassword" : "Password for hello file containing certificate and private key"
            }
          }

### <a name="alert-logic-log-manager"></a><span data-ttu-id="244f3-142">Riasztási logika naplókezelő</span><span class="sxs-lookup"><span data-stu-id="244f3-142">Alert Logic Log Manager</span></span>
          {
            "publisher": "AlertLogic.Extension",
            "type": "AlertLogicLM",
            "typeHandlerVersion": "1.9",
            "settings": {
              "registrationKey" : " Alert Logic Log Manager registration key"
            }
          }

### <a name="chef-agent"></a><span data-ttu-id="244f3-143">Chef ügynök</span><span class="sxs-lookup"><span data-stu-id="244f3-143">Chef Agent</span></span>
          {
            "publisher": "Chef.Bootstrap.WindowsAzure",
            "type": "ChefClient",
            "typeHandlerVersion": "1210.12",
            "settings": {
              "validation_key" : " Validation key",
              "client_rb" : "client_rb file",
              "runlist" : "Optional runlist"
            }
          }

### <a name="azure-diagnostics"></a><span data-ttu-id="244f3-144">Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="244f3-144">Azure Diagnostics</span></span>
<span data-ttu-id="244f3-145">További részletes információt tooconfigure diagnosztika, lásd: [Azure Diagnostics bővítmény](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="244f3-145">For more details about how tooconfigure diagnostics, see [Azure Diagnostics Extension](extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)</span></span>

          {
            "publisher": "Microsoft.Azure.Diagnostics",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "1.5",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "xmlCfg": "[base64(variables('wadcfgx'))]",
              "storageAccount": "[parameters('diagnosticsStorageAccount')]"
            },
            "protectedSettings": {
            "storageAccountName": "[parameters('diagnosticsStorageAccount')]",
            "storageAccountKey": "[listkeys(variables('accountid'), '2015-05-01-preview').key1]",
            "storageAccountEndPoint": "https://core.windows.net"
          }
          }

### <a name="octopus-deploy-tentacle-agent"></a><span data-ttu-id="244f3-146">Polip Tentacle-ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="244f3-146">Octopus Deploy Tentacle Agent</span></span>

<span data-ttu-id="244f3-147">Hogyan tooconfigure hello Azure Polip telepítése Tentacle kapcsolatos további tudnivalókért lásd: hello [Polip dokumentáció](https://octopus.com/docs/installation/installing-tentacles/azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="244f3-147">For more details about how tooconfigure hello Octopus Deploy Tentacle on Azure, see hello [Octopus Documentation](https://octopus.com/docs/installation/installing-tentacles/azure-virtual-machines).</span></span>

          {
            "publisher": "OctopusDeploy.Tentacle",
            "type": "OctopusDeployWindowsTentacle",
            "typeHandlerVersion": "2.0",
            "autoUpgradeMinorVersion": "true",
            "settings": {
              "OctopusServerUrl": "(string, required) hello url toohello Octopus server portal.",
              "Environments": [ "(array of strings, required) hello environments toowhich hello Tentacle should be added." ],
              "Roles": [ "(array of strings, required) hello roles tooassign toohello Tentacle." ],
              "CommunicationMode": "(string, required) Whether hello Tentacle should wait for connections from hello server ('Listen') or should poll hello server ('Poll').",
              "Port": (int, required) hello port toolisten on for connections from hello server (in 'Listen' mode), or hello port on which tooconnect toohello Octopus server ('Poll' mode).,
              "PublicHostNameConfiguration": "(string, optional) If in listening mode, how hello server should contact hello Tentacle. Can be 'PublicIP', 'FQDN', 'ComputerName' or 'Custom'. Defaults too'PublicIp'.",
              "CustomPublicHostName": "(string, optional) If in listening mode, and 'PublicHostNameConfiguration' is set too'Custom', hello address that hello server should use for this Tentacle.",
              "MachinePolicy": "(string, optional) hello Machine Policy tooassign toohello Tentacle. If not specified, uses hello default Machine Policy.",
              "Tenants": [ "(array of strings, optional) hello tenants tooassign toohello Tentacle. hello tenants feature must be enabled on hello Octopus Server." ],
              "TenantTags": [ "(array of strings, optional) hello tenant tags tooassign toohello Tentacle, in hello format 'TagSet/TagName'. hello tenants feature must be enabled on hello Octopus Server." ]
            },
            "protectedSettings": {
              "ApiKey": "(string, required) hello Api Key toouse tooconnect toohello Octopus server."
            }
          }

<span data-ttu-id="244f3-148">Hello példában cserélje le hello verziószám hello legújabb verziószámot.</span><span class="sxs-lookup"><span data-stu-id="244f3-148">In hello examples above, replace hello version number with hello latest version number.</span></span>

<span data-ttu-id="244f3-149">Íme egy példa az egyéni parancsprogramok futtatására szolgáló bővítmény teljes Virtuálisgép-sablont.</span><span class="sxs-lookup"><span data-stu-id="244f3-149">Here is an example of a full VM template with Custom Script Extension.</span></span>

[<span data-ttu-id="244f3-150">Virtuális gép Windows egyéni parancsprogramok futtatására szolgáló bővítmény</span><span class="sxs-lookup"><span data-stu-id="244f3-150">Custom Script Extension on a Windows VM</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)

