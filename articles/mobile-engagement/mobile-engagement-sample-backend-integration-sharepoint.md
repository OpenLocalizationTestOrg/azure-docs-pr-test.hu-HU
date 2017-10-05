---
title: "Az Azure Mobile Engagement - háttér-integráció"
description: "Csatlakozás az Azure Mobile Engagement egy SharePoint-háttérrendszerrel SharePoint kampányok létrehozásához"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 06297b43-579f-46e6-8a58-961a68f9aa09
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: d49f1094f4c3f170f3618f3e19e42266f9ae8858
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-mobile-engagement---api-integration"></a><span data-ttu-id="9de81-103">Az Azure Mobile Engagement - API-integráció</span><span class="sxs-lookup"><span data-stu-id="9de81-103">Azure Mobile Engagement - API integration</span></span>
<span data-ttu-id="9de81-104">Egy automatizált marketing rendszerben létrehozása és aktiválása, a marketingkampányok is automatikusan történik.</span><span class="sxs-lookup"><span data-stu-id="9de81-104">In an automated marketing system, creating and activating the marketing campaigns also occur automatically.</span></span> <span data-ttu-id="9de81-105">Erre a célra - Azure Mobile Engagement lehetővé teszi, hogy az ilyen API-k használatával is automatizált marketingkampányok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9de81-105">For this purpose - Azure Mobile Engagement enables creating such automated marketing campaigns using APIs as well.</span></span> 

<span data-ttu-id="9de81-106">Általában az ügyfelek a Mobile Engagement előtér illesztő segítségével a marketingkampányok részeként hozza létre a közlemények/szavazások stb.</span><span class="sxs-lookup"><span data-stu-id="9de81-106">Typically customers use the Mobile Engagement front end interface to create announcements/polls etc as part of their marketing campaigns.</span></span> <span data-ttu-id="9de81-107">Azonban a marketingkampányok egyre érett, szükség van az adatokat, hogy egy teljesen automatizált folyamat is létrehozható, amely kampányok hoz létre a Mobile Engagement zárolva van (például a CRM vagy CMS rendszer például SharePoint) a háttérrendszerek ki dinamikusan áramlanak a háttérrendszerek adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="9de81-107">However as the marketing campaigns become mature, there is a need to leverage the data locked in the backend systems (like any CRM system or CMS system like SharePoint) so that a fully automated pipeline can be created which creates campaigns in Mobile Engagement dynamically based on the data flowing in from the backend systems.</span></span> 

![][5]

<span data-ttu-id="9de81-108">Ez az oktatóanyag egy olyan forgatókönyvet, ahol a SharePoint üzleti felhasználók és tölti fel az értékesítési adatokat egy SharePoint-lista egy automatikus folyamat szerzi be a listában szereplő elemeket, és csatlakozik a Mobile Engagement rendszer használata a rendelkezésre álló REST API-k létrehozásához az végig kell vinnie a a SharePoint-adatokból marketingkampányt.</span><span class="sxs-lookup"><span data-stu-id="9de81-108">This tutorial goes through such a scenario where a SharePoint business user populates a SharePoint list with marketing data and an automated process picks up items from the list and connects with the Mobile Engagement system using the available REST APIs to create a marketing campaign from the SharePoint data.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="9de81-109">Általában használhatja ezt a mintát kiindulási pontként a Mobile Engagement REST API-k előhívhat, hogy az adatokat a két fő szempontjait hívja az API - hitelesítő és tompított paraméterek alapos ismerete.</span><span class="sxs-lookup"><span data-stu-id="9de81-109">In general, you can use this sample as a starting point for understanding how to call any Mobile Engagement REST API as it details the two key aspects of calling the APIs - authenticating and passing parameters.</span></span> 
> 
> 

## <a name="sharepoint-integration"></a><span data-ttu-id="9de81-110">SharePoint-integráció</span><span class="sxs-lookup"><span data-stu-id="9de81-110">SharePoint integration</span></span>
1. <span data-ttu-id="9de81-111">Ez a minta SharePoint-lista néz.</span><span class="sxs-lookup"><span data-stu-id="9de81-111">Here is what the sample SharePoint list looks like.</span></span> <span data-ttu-id="9de81-112">**Cím**, **kategória**, **NotificationTitle**, **üzenet** és **URL-cím** a hirdetmény létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="9de81-112">**Title**, **Category**, **NotificationTitle**, **Message** and **URL** are used for creating the announcement.</span></span> <span data-ttu-id="9de81-113">Nincs oszlopát **IsProcessed** konzol program formájában a minta automation folyamat által használt.</span><span class="sxs-lookup"><span data-stu-id="9de81-113">There is a column called **IsProcessed** which is used by the sample automation process in the form of a console program.</span></span> <span data-ttu-id="9de81-114">Akkor is fut a konzol, az Azure webjobs-feladat program, így is ütemezheti, vagy használhatja a SharePoint-munkafolyamat létrehozása és aktiválása a közlemény, amikor egy elem bekerülnek a SharePoint-lista program közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="9de81-114">You can either run this console program as an Azure WebJob so that you can schedule it or you can directly use the SharePoint workflow to program creating and activating the announcement when an item is inserted into the SharePoint list.</span></span> <span data-ttu-id="9de81-115">Ez a példa a konzol programot, amely a SharePoint elemeinek listában, és bejelentés létrehozása az Azure Mobile Engagement esetében kerül, és megjelöli végül használjuk a **IsProcessed** jelző IGAZ értékű, a sikeres bejelentés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9de81-115">In this sample we use the console program which goes through the items in the SharePoint list and create announcement in Azure Mobile Engagement for each of them and then finally marks the **IsProcessed** flag to be true on successful announcement creation.</span></span>
   
    ![][1]
2. <span data-ttu-id="9de81-116">A kód a minta használjuk *távoli hitelesítés a SharePoint Online használata az ügyfél objektummodell* [Itt](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) való hitelesítéshez szükséges a SharePoint-listán.</span><span class="sxs-lookup"><span data-stu-id="9de81-116">We are using the code from the sample *Remote Authentication in SharePoint Online Using the Client Object Model* [here](https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c) to authenticate with the SharePoint list.</span></span>
3. <span data-ttu-id="9de81-117">Hitelesítését követően azt ismétlése minden újonnan létrehozott elemek megállapítása a listaelemek (amelyeket lesz **IsProcessed** = false).</span><span class="sxs-lookup"><span data-stu-id="9de81-117">Once authenticated, we loop through the list items to find out any newly created items (which will have **IsProcessed** = false).</span></span> 
   
         static async void CreateCampaignFromSharepoint()
        {
            using (ClientContext clientContext = ClaimClientContext.GetAuthenticatedContext(targetSharepointSite))
            {
                // Using https://code.msdn.microsoft.com/Remote-Authentication-in-b7b6f43c for authentication     
                Web site = clientContext.Web;
                List list = site.Lists.GetByTitle("VideoContent");
                CamlQuery query = new CamlQuery();
                query.ViewXml = "<View/>";
                ListItemCollection items = list.GetItems(query);
   
                // Load the SharePoint list
                clientContext.Load(list);
                clientContext.Load(items);
                clientContext.ExecuteQuery();
   
                // Loop through the list to go through all the items which are newly added
                foreach (ListItem item in items)
                    if (item["IsProcessed"].ToString() != "Yes")
                    {
                        string name = item["Title"].ToString();
                        string notificationTitle = item["NotificationTitle"].ToString();
                        string notificationMessage = item["Message"].ToString();
                        string category = item["Category"].ToString();
                        string actionURL = ((FieldUrlValue)item["URL"]).Url;
   
                        // Send an HTTP request to create AzME campaign
                        int campaignId = CreateAzMECampaign
                            (name, notificationTitle, notificationMessage, category, actionURL).Result;
   
                        if (campaignId != -1)
                        {
                            // If creating campaign is successful then set this to true
                            item["IsProcessed"] = "Yes";
   
                            // Now try to activate the campaign also
                            await ActivateAzMECampaign(campaignId);
                        }
                        else
                        {
                            item["IsProcessed"] = "Error";
                        }
                        item.Update();
                    }
                clientContext.ExecuteQuery();
            }
        }

## <a name="mobile-engagement-integration"></a><span data-ttu-id="9de81-118">Mobile Engagement-integráció</span><span class="sxs-lookup"><span data-stu-id="9de81-118">Mobile Engagement integration</span></span>
1. <span data-ttu-id="9de81-119">Amint egy elem feldolgozása - igénylő találtunk azt bontsa ki a közlemény létrehozása a listaelemek és hívás szükséges adatokat `CreateAzMECampaign` létrehozták, és ezt követően `ActivateAzMECampaign` aktiválásához.</span><span class="sxs-lookup"><span data-stu-id="9de81-119">Once we find an item which requires processing - we extract the information required to create an announcement from the list item and call `CreateAzMECampaign` to create it and subsequently `ActivateAzMECampaign` to activate it.</span></span> <span data-ttu-id="9de81-120">Ezek a lényegében REST API-hívásokat a Mobile Engagement háttérrendszeréhez hívása.</span><span class="sxs-lookup"><span data-stu-id="9de81-120">These are essentially REST API calls calling to the Mobile Engagement backend.</span></span> 
2. <span data-ttu-id="9de81-121">A Mobile Engagement REST API-k szükséges egy **alapszintű hitelesítési séma engedélyezési HTTP-fejléc** amely áll a `ApplicationId` és a `ApiKey` amely kap az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9de81-121">The Mobile Engagement REST APIs require a **Basic auth scheme authorization HTTP header** which is composed of the `ApplicationId` and the `ApiKey` which you get from the Azure portal.</span></span> <span data-ttu-id="9de81-122">Győződjön meg arról, hogy a kulcsot használ a **api-kulcsokat** szakasz és *nem* a a **sdk kulcsok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="9de81-122">Make sure that you are using the Key from the **api keys** section and *not* from the **sdk keys** section.</span></span> 
   
   ![][2]
   
       static string CreateAuthZHeader()
       {
           string BasicAuthzHeaderString = "Basic " + EncodeTo64(ApplicationId + ":" + ApiKey);
           return BasicAuthzHeaderString;
       }
   
       static public string EncodeTo64(string toEncode)
       {
           byte[] toEncodeAsBytes = System.Text.ASCIIEncoding.ASCII.GetBytes(toEncode);
           string returnValue = System.Convert.ToBase64String(toEncodeAsBytes);
           return returnValue;
       }  
3. <span data-ttu-id="9de81-123">A hirdetmény létrehozásához írja be a kampány – tekintse meg a [dokumentáció](https://msdn.microsoft.com/library/azure/mt683750.aspx).</span><span class="sxs-lookup"><span data-stu-id="9de81-123">For creating the announcement type campaign - refer to the [documentation](https://msdn.microsoft.com/library/azure/mt683750.aspx).</span></span> <span data-ttu-id="9de81-124">Győződjön meg arról, hogy a kampány ad meg kell `kind` , *közlemény* és a [hasznos](https://msdn.microsoft.com/library/azure/mt683751.aspx) és FormUrlEncodedContent, átadja azt.</span><span class="sxs-lookup"><span data-stu-id="9de81-124">You need to make sure that you are specifying the campaign `kind` as *announcement* and the [payload](https://msdn.microsoft.com/library/azure/mt683751.aspx) and passing it as FormUrlEncodedContent.</span></span> 
   
        static async Task<int> CreateAzMECampaign(string campaignName, string notificationTitle, 
            string notificationMessage, string notificationCategory, string actionURL)
        {
            string createURIFragment = "/reach/1/create";
   
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
   
                // Create the payload to send the content
                // Reference -> https://msdn.microsoft.com/library/dn913749.aspx
                string data =
                    @"{""name"":""" + campaignName + @"""," + 
                    @"""type"":""only_notif""," + 
                    @"""deliveryTime"":""any"","" + 
                    @""notificationTitle"":""" + notificationTitle + 
                    @""",""notificationMessage"":""" + notificationMessage + 
                    @""",""actionUrl"":""" + actionURL + @"""}";
   
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("data", data),
                });
   
                // Send the POST request
                var response = await client.PostAsync(url + createURIFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                int campaignId = -1;
                if (response.StatusCode.ToString() == "OK")
                {
                    // This is the campaign id
                    campaignId = Convert.ToInt32(responseString);
                    Console.WriteLine("Campaign successfully created with id {0}", campaignId);
                }
                else
                {
                    Console.WriteLine("Campaign creation failed with error - '{0}'", responseString);
                }
                return campaignId;
            }
        }
4. <span data-ttu-id="9de81-125">Ha már létrehozott bejelentés, megjelenik a következőhöz a Mobile Engagement portál (vegye figyelembe, hogy az állapot = Vázlat és aktív = N/A)</span><span class="sxs-lookup"><span data-stu-id="9de81-125">Once you have the announcement created, you will see something like the following on the Mobile Engagement portal (note that the State=Draft and Activated = N/A)</span></span>
   
    ![][3]
5. <span data-ttu-id="9de81-126">`CreateAzMECampaign`létrehoz egy bejelentési kampányt, és visszaadja az azonosítója a hívó.</span><span class="sxs-lookup"><span data-stu-id="9de81-126">`CreateAzMECampaign` creates an announcement campaign and returns its Id to the caller.</span></span> <span data-ttu-id="9de81-127">`ActivateAzMECampaign`Ez az azonosító szükséges aktiválja a kampányt paraméterként.</span><span class="sxs-lookup"><span data-stu-id="9de81-127">`ActivateAzMECampaign` requires this Id as the parameter to activate the campaign.</span></span> 
   
        static async Task<bool> ActivateAzMECampaign(int campaignId)
        {
            string activateUriFragment = "/reach/1/activate";
            using (var client = new HttpClient())
            {
                // Add Authorization Header
                client.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", CreateAuthZHeader());
   
                var content = new FormUrlEncodedContent(new[] 
                {
                    new KeyValuePair<string, string>("kind", "announcement"),
                    new KeyValuePair<string, string>("id", campaignId.ToString()),
                });
   
                // Send the POST request
                var response = await client.PostAsync(url + activateUriFragment, content);
                var responseString = response.Content.ReadAsStringAsync().Result;
                if (response.StatusCode.ToString() == "OK")
                {
                    Console.WriteLine("Campaign successfully activated");
                    return true;
                }
                else
                {
                    Console.WriteLine("Campaign activation failed with error - '{0}'", responseString);
                    return false;
                }
            }
        }
6. <span data-ttu-id="9de81-128">Ha már aktiválta a közlemény, a Mobile Engagement portálon megjelenik a következőhöz:</span><span class="sxs-lookup"><span data-stu-id="9de81-128">Once you have the announcement activated, you will see something like the following on the Mobile Engagement portal:</span></span>
   
    ![][4]
7. <span data-ttu-id="9de81-129">Amint az a kampány aktiválása lekérdezi azokat az eszközöket, amelyek megfelelnek a feltétel a kampány indul el értesítések jelent.</span><span class="sxs-lookup"><span data-stu-id="9de81-129">As soon as the campaign gets activated, any devices which satisfy the criterion on the campaign will start seeing notifications.</span></span> 
8. <span data-ttu-id="9de81-130">Is láthatja, hogy a listaelem jelölésű IsProcessed = hamis értéket Igaz értékre lett állítva a közlemény kampány létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="9de81-130">You will also notice that the list item marked with IsProcessed = false has been set to True once the announcement campaign is created.</span></span>  

<span data-ttu-id="9de81-131">Ez a minta legtöbbször a szükséges tulajdonságok megadása egyszerű közlemény a kampány létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9de81-131">This sample created a simple announcement campaign specifying mostly the required properties.</span></span> <span data-ttu-id="9de81-132">Testre szabhatja ezt, mint amennyit a portálról szereplő információk segítségével is [Itt](https://msdn.microsoft.com/library/azure/mt683751.aspx).</span><span class="sxs-lookup"><span data-stu-id="9de81-132">You can customize this as much as you can from the portal by using the information [here](https://msdn.microsoft.com/library/azure/mt683751.aspx).</span></span> 

<!-- Images. -->
[1]: ./media/mobile-engagement-sample-backend-integration-sharepoint/sharepointlist.png
[2]: ./media/mobile-engagement-sample-backend-integration-sharepoint/properties.png
[3]: ./media/mobile-engagement-sample-backend-integration-sharepoint/new-announcement.png
[4]: ./media/mobile-engagement-sample-backend-integration-sharepoint/activate-announcement.png
[5]: ./media/mobile-engagement-sample-backend-integration-sharepoint/diagram.png



