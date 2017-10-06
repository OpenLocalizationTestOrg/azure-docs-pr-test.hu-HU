---
title: "aaaGet lépések a Visual Studio MVC-projektek az Azure AD-val |} Microsoft Docs"
description: "Hogyan tooget el az Azure Active Directoryval MVC projekt létrehozása a Visual Studio használatával Azure AD tooor csatlakoztatása után kapcsolódó szolgáltatások"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1c8b6a58-5144-4965-a905-625b9ee7b22b
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 807824dd6e4e57e443f8a7322cf2e5326384316d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-active-directory-and-visual-studio-connected-services-mvc-projects"></a>Ismerkedés az Azure Active Directory és a Visual Studio csatlakoztatva (MVC projektek) szolgáltatások
> [!div class="op_single_selector"]
> * [Első lépések](vs-active-directory-dotnet-getting-started.md)
> * [mi történt](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="requiring-authentication-tooaccess-controllers"></a>Hitelesítési tooaccess tartományvezérlők megkövetelése
A projekt lévő összes volt adorned a hello **engedélyezés** attribútum. Ez az attribútum szükséges hello felhasználói toobe hitelesített ezen vezérlők elérése előtt. tooallow hello vezérlő toobe érhető el névtelen hozzáféréssel, ez az attribútum eltávolítása hello vezérlőről. Ha azt szeretné, hogy részletesebb szinten tooset hello engedélyek, alkalmazza a hello attribútum tooeach módszer, amely engedélyezési helyett toohello vezérlőosztály telepítené azt.

## <a name="adding-signin--signout-controls"></a>Bejelentkezés felvétele / SignOut szabályozza
tooadd hello SignIn/SignOut tooyour nézet szabályozza, használhatja a hello **_LoginPartial.cshtml** részleges nézet tooadd hello funkció tooone a nézetek. Példa hello funkció hozzáadott toohello standard **_Layout.cshtml** nézet. (Megjegyzés: hello utolsó elem az hello div rendelkező osztály navigációs sávja összecsukása):

<pre>
    &lt;!DOCTYPE html&gt; 
     &lt;html&gt; 
     &lt;head&gt; 
         &lt;meta charset="utf-8" /&gt; 
        &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt; 
        &lt;title&gt;@ViewBag.Title - My ASP.NET Application&lt;/title&gt; 
        @Styles.Render("~/Content/css") 
        @Scripts.Render("~/bundles/modernizr") 
    &lt;/head&gt; 
    &lt;body&gt; 
        &lt;div class="navbar navbar-inverse navbar-fixed-top"&gt; 
            &lt;div class="container"&gt; 
                &lt;div class="navbar-header"&gt; 
                    &lt;button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse"&gt; 
                        &lt;span class="icon-bar"&gt;&lt;/span&gt; 
                        &lt;span class="icon-bar"&gt;&lt;/span&gt; 
                        &lt;span class="icon-bar"&gt;&lt;/span&gt; 
                    &lt;/button&gt; 
                    @Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" }) 
                &lt;/div&gt; 
                &lt;div class="navbar-collapse collapse"&gt; 
                    &lt;ul class="nav navbar-nav"&gt; 
                        &lt;li&gt;@Html.ActionLink("Home", "Index", "Home")&lt;/li&gt; 
                        &lt;li&gt;@Html.ActionLink("About", "About", "Home")&lt;/li&gt; 
                        &lt;li&gt;@Html.ActionLink("Contact", "Contact", "Home")&lt;/li&gt; 
                    &lt;/ul&gt; 
                    <span style="background-color:yellow">@Html.Partial("_LoginPartial")</span> 
                &lt;/div&gt; 
            &lt;/div&gt; 
        &lt;/div&gt; 
        &lt;div class="container body-content"&gt; 
            @RenderBody() 
            &lt;hr /&gt; 
            &lt;footer&gt; 
                &lt;p&gt;&amp;copy; @DateTime.Now.Year - My ASP.NET Application&lt;/p&gt; 
            &lt;/footer&gt; 
        &lt;/div&gt; 
        @Scripts.Render("~/bundles/jquery") 
        @Scripts.Render("~/bundles/bootstrap") 
        @RenderSection("scripts", required: false) 
    &lt;/body&gt; 
    &lt;/html&gt;
</pre>

## <a name="next-steps"></a>Következő lépések
- [További tudnivalók az Azure Active Directoryban](https://azure.microsoft.com/services/active-directory/) 

