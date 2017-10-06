---
title: "aaaAzure Active Directory B2B együttműködés kód és a PowerShell-példák |} Microsoft Docs"
description: "Azure Active Directory B2B együttműködés kód és a PowerShell-példák"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/11/2017
ms.author: sasubram
ms.openlocfilehash: 8e4f66fcb50d190899304831ea7ccd2203c5468c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2b-collaboration-code-and-powershell-samples"></a><span data-ttu-id="44028-103">Az Azure Active Directory B2B együttműködés kód és a PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="44028-103">Azure Active Directory B2B collaboration code and PowerShell samples</span></span>

## <a name="powershell-example"></a><span data-ttu-id="44028-104">PowerShell – példa</span><span class="sxs-lookup"><span data-stu-id="44028-104">PowerShell example</span></span>
<span data-ttu-id="44028-105">Tömeges-összehíváshoz külső felhasználók tooan szervezet e-mail címet, amelyekre tárolt is egy. CSV-fájl.</span><span class="sxs-lookup"><span data-stu-id="44028-105">You can bulk-invite external users tooan organization from email addresses that you have stored in a .CSV file.</span></span>

1. <span data-ttu-id="44028-106">Készítse elő a hello. Fürt megosztott kötetei szolgáltatás fájlt hozzon létre egy új CSV-fájlt, és adjon neki nevet invitations.csv.</span><span class="sxs-lookup"><span data-stu-id="44028-106">Prepare hello .CSV file Create a new CSV file and name it invitations.csv.</span></span> <span data-ttu-id="44028-107">Ebben a példában a hello fájl, C:\Data mentett, és tartalmazza a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="44028-107">In this example, hello file is saved in C:\data, and contains hello following information:</span></span>
  
  <span data-ttu-id="44028-108">Név</span><span class="sxs-lookup"><span data-stu-id="44028-108">Name</span></span>                  |  <span data-ttu-id="44028-109">InvitedUserEmailAddress</span><span class="sxs-lookup"><span data-stu-id="44028-109">InvitedUserEmailAddress</span></span>
  --------------------- | --------------------------
  <span data-ttu-id="44028-110">Gmail B2B meghívott</span><span class="sxs-lookup"><span data-stu-id="44028-110">Gmail B2B Invitee</span></span>     | b2binvitee@gmail.com
  <span data-ttu-id="44028-111">Outlook B2B meghívott</span><span class="sxs-lookup"><span data-stu-id="44028-111">Outlook B2B invitee</span></span>   | b2binvitee@outlook.com


2. <span data-ttu-id="44028-112">Első hello legújabb Azure AD PowerShell toouse hello új parancsmagok, telepítenie kell a frissített hello Azure AD PowerShell modult, amely letölthető [hello Powershell-modul kiadás lap](https://www.powershellgallery.com/packages/AzureADPreview)</span><span class="sxs-lookup"><span data-stu-id="44028-112">Get hello latest Azure AD PowerShell toouse hello new cmdlets, you must install hello updated Azure AD PowerShell module, which you can download from [hello Powershell module's release page](https://www.powershellgallery.com/packages/AzureADPreview)</span></span>

3. <span data-ttu-id="44028-113">Jelentkezzen be tooyour bérlős működés támogatása</span><span class="sxs-lookup"><span data-stu-id="44028-113">Sign in tooyour tenancy</span></span>

    ```
    $cred = Get-Credential
    Connect-AzureAD -Credential $cred
    ```

4. <span data-ttu-id="44028-114">Hello PowerShell parancsmag futtatása</span><span class="sxs-lookup"><span data-stu-id="44028-114">Run hello PowerShell cmdlet</span></span>

  ```
  $invitations = import-csv C:\data\invitations.csv
  $messageInfo = New-Object Microsoft.Open.MSGraph.Model.InvitedUserMessageInfo
  $messageInfo.customizedMessageBody = “Hey there! Check this out. I created an invitation through PowerShell”
  foreach ($email in $invitations) {New-AzureADMSInvitation -InvitedUserEmailAddress $email.InvitedUserEmailAddress -InvitedUserDisplayName $email.Name -InviteRedirectUrl https://wingtiptoysonline-dev-ed.my.salesforce.com -InvitedUserMessageInfo $messageInfo -SendInvitationMessage $true}
  ```

<span data-ttu-id="44028-115">Ez a parancsmag elküldi meghívót toohello e-mail címek invitations.csv.</span><span class="sxs-lookup"><span data-stu-id="44028-115">This cmdlet sends an invitation toohello email addresses in invitations.csv.</span></span> <span data-ttu-id="44028-116">Ez a parancsmag a további funkciók a következők:</span><span class="sxs-lookup"><span data-stu-id="44028-116">Additional features of this cmdlet include:</span></span>
- <span data-ttu-id="44028-117">Testre szabott szöveg hello e-mail üzenetben</span><span class="sxs-lookup"><span data-stu-id="44028-117">Customized text in hello email message</span></span>
- <span data-ttu-id="44028-118">Többek között a hello megjelenítési nevet meghívott felhasználó</span><span class="sxs-lookup"><span data-stu-id="44028-118">Including a display name for hello invited user</span></span>
- <span data-ttu-id="44028-119">TooCCs üzenetek küldésekor vagy e-mailek teljesen kikapcsolása</span><span class="sxs-lookup"><span data-stu-id="44028-119">Sending messages tooCCs or suppressing email messages altogether</span></span>

## <a name="code-sample"></a><span data-ttu-id="44028-120">Kódminta</span><span class="sxs-lookup"><span data-stu-id="44028-120">Code sample</span></span>
<span data-ttu-id="44028-121">Itt azt mutatják be, hogyan toocall hello meghívó API, "csak alkalmazás" módban, tooget hello érvényesítési URL-címe hello erőforrás toowhich fióknevet hello B2B felhasználó.</span><span class="sxs-lookup"><span data-stu-id="44028-121">Here we illustrate how toocall hello invitation API, in "app-only" mode, tooget hello redemption URL for hello resource toowhich you are inviting hello B2B user.</span></span> <span data-ttu-id="44028-122">hello célja egy egyéni meghívó e-mail toosend.</span><span class="sxs-lookup"><span data-stu-id="44028-122">hello goal is toosend a custom invitation email.</span></span> <span data-ttu-id="44028-123">hello e-mail összeállítható egy HTTP-ügyféllel, így testre szabhatja a megjelenését, és elküldi a Graph API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="44028-123">hello email can be composed with an HTTP client, so you can customize how it looks and send it through Graph API.</span></span>

```
namespace SampleInviteApp
{
    using System;
    using System.Linq;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Newtonsoft.Json;
    class Program
    {
        /// <summary>
        /// Microsoft graph resource.
        /// </summary>
        static readonly string GraphResource = "https://graph.microsoft.com";
 
        /// <summary>
        /// Microsoft graph invite endpoint.
        /// </summary>
        static readonly string InviteEndPoint = "https://graph.microsoft.com/v1.0/invitations";
 
        /// <summary>
        ///  Authentication endpoint tooget token.
        /// </summary>
        static readonly string EstsLoginEndpoint = "https://login.microsoftonline.com";
 
        /// <summary>
        /// This is hello tenantid of hello tenant you want tooinvite users to.
        /// </summary>
        private static readonly string TenantID = "";
 
        /// <summary>
        /// This is hello application id of hello application that is registered in hello above tenant.
        /// hello required scopes are available in hello below link.
        /// https://developer.microsoft.com/graph/docs/api-reference/v1.0/api/invitation_post
        /// </summary>
        private static readonly string TestAppClientId = "";
 
        /// <summary>
        /// Client secret of hello application.
        /// </summary>
        private static readonly string TestAppClientSecret = @"
 
        /// <summary>
        /// This is hello email address of hello user you want tooinvite.
        /// </summary>
        private static readonly string InvitedUserEmailAddress = @"";
 
        /// <summary>
        /// This is hello display name of hello user you want tooinvite.
        /// </summary>
        private static readonly string InvitedUserDisplayName = @"";
 
        /// <summary>
        /// Main method.
        /// </summary>
        /// <param name="args">Optional arguments</param>
        static void Main(string[] args)
        {
            Invitation invitation = CreateInvitation();
            SendInvitation(invitation);
        }
 
        /// <summary>
        /// Create hello invitation object.
        /// </summary>
        /// <returns>Returns hello invitation object.</returns>
        private static Invitation CreateInvitation()
        {
            // Set hello invitation object.
            Invitation invitation = new Invitation();
            invitation.InvitedUserDisplayName = InvitedUserDisplayName;
            invitation.InvitedUserEmailAddress = InvitedUserEmailAddress;
            invitation.InviteRedirectUrl = "https://www.microsoft.com";
            invitation.SendInvitationMessage = true;
            return invitation;
        }
 
        /// <summary>
        /// Send hello guest user invite request.
        /// </summary>
        /// <param name="invitation">Invitation object.</param>
        private static void SendInvitation(Invitation invitation)
        {
            string accessToken = GetAccessToken();
 
            HttpClient httpClient = GetHttpClient(accessToken);
 
            // Make hello invite call. 
            HttpContent content = new StringContent(JsonConvert.SerializeObject(invitation));
            content.Headers.Add("ContentType", "application/json");
            var postResponse = httpClient.PostAsync(InviteEndPoint, content).Result;
            string serverResponse = postResponse.Content.ReadAsStringAsync().Result;
            Console.WriteLine(serverResponse);
        }
 
        /// <summary>
        /// Get hello HTTP client.
        /// </summary>
        /// <param name="accessToken">Access token</param>
        /// <returns>Returns hello Http Client.</returns>
        private static HttpClient GetHttpClient(string accessToken)
        {
            // setup http client.
            HttpClient httpClient = new HttpClient();
            httpClient.Timeout = TimeSpan.FromSeconds(300);
            httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", accessToken);
            httpClient.DefaultRequestHeaders.Add("client-request-id", Guid.NewGuid().ToString());
            Console.WriteLine(
                "CorrelationID for hello request: {0}",
                httpClient.DefaultRequestHeaders.GetValues("client-request-id").Single());
            return httpClient;
        }
 
        /// <summary>
        /// Get hello access token for our application tootalk toomicrosoft graph.
        /// </summary>
        /// <returns>Returns hello access token for our application tootalk toomicrosoft graph.</returns>
        private static string GetAccessToken()
        {
            string accessToken = null;
 
            // Get hello access token for our application tootalk toomicrosoft graph.
            try
            {
                AuthenticationContext testAuthContext =
                    new AuthenticationContext(string.Format("{0}/{1}", EstsLoginEndpoint, TenantID));
                AuthenticationResult testAuthResult = testAuthContext.AcquireTokenAsync(
                    GraphResource,
                    new ClientCredential(TestAppClientId, TestAppClientSecret)).Result;
                accessToken = testAuthResult.AccessToken;
            }
            catch (AdalException ex)
            {
                Console.WriteLine("An exception was thrown while fetching hello token: {0}.", ex);
                throw;
            }
 
            return accessToken;
        }
 
        /// <summary>
        /// Invitation class.
        /// </summary>
        public class Invitation
        {
            /// <summary>
            /// Gets or sets display name.
            /// </summary>
            public string InvitedUserDisplayName { get; set; }
 
            /// <summary>
            /// Gets or sets display name.
            /// </summary>
            public string InvitedUserEmailAddress { get; set; }
 
            /// <summary>
            /// Gets or sets a value indicating whether Invitation Manager should send hello email tooInvitedUser.
            /// </summary>
            public bool SendInvitationMessage { get; set; }
 
            /// <summary>
            /// Gets or sets invitation redirect URL
            /// </summary>
            public string InviteRedirectUrl { get; set; }
        }
    }
}
```


## <a name="next-steps"></a><span data-ttu-id="44028-124">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="44028-124">Next steps</span></span>

<span data-ttu-id="44028-125">Ismerje meg az Azure AD B2B együttműködés további cikkeit:</span><span class="sxs-lookup"><span data-stu-id="44028-125">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="44028-126">Mi az az Azure AD B2B együttműködés?</span><span class="sxs-lookup"><span data-stu-id="44028-126">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="44028-127">B2B együttműködés felhasználó tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="44028-127">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="44028-128">B2B együttműködés felhasználói tooa szerepkör hozzáadása</span><span class="sxs-lookup"><span data-stu-id="44028-128">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="44028-129">B2B együttműködés meghívókat delegálása</span><span class="sxs-lookup"><span data-stu-id="44028-129">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="44028-130">Dinamikus csoportok és a B2B együttműködés</span><span class="sxs-lookup"><span data-stu-id="44028-130">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="44028-131">B2B együttműködés SaaS-alkalmazások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="44028-131">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="44028-132">B2B együttműködés felhasználói jogkivonatokhoz</span><span class="sxs-lookup"><span data-stu-id="44028-132">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="44028-133">B2B együttműködés felhasználói jogcímek leképezése</span><span class="sxs-lookup"><span data-stu-id="44028-133">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="44028-134">Külső Office 365-megosztás</span><span class="sxs-lookup"><span data-stu-id="44028-134">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="44028-135">B2B együttműködés aktuális korlátozásai</span><span class="sxs-lookup"><span data-stu-id="44028-135">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
