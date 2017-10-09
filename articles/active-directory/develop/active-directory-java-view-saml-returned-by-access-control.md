---
title: "Hozzáférés-vezérlési szolgáltatásban (Java) hello SAML visszaadott aaaView"
description: "Ismerje meg, hogyan tooview hello hozzáférés-vezérlési szolgáltatásban a Java-alkalmazások által visszaadott SAML Azure-platformon futó."
services: active-directory
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 6cd216f9-eb43-46b4-b30d-f194d0ae2d48
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.custom: aaddev
ms.openlocfilehash: b6733bc98b505cfa89a4ce456f368ee15da11427
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooview-saml-returned-by-hello-azure-access-control-service"></a><span data-ttu-id="5d8ef-103">Hogyan tooview SAML hello Azure Access Control Service által visszaadott</span><span class="sxs-lookup"><span data-stu-id="5d8ef-103">How tooview SAML returned by hello Azure Access Control Service</span></span>
<span data-ttu-id="5d8ef-104">Ez az útmutató bemutatja, hogyan tooview hello az alapul szolgáló Security Assertion Markup Language (SAML) tooyour alkalmazás által visszaadott hello Azure Access Control Service (ACS).</span><span class="sxs-lookup"><span data-stu-id="5d8ef-104">This guide will show you how tooview hello underlying Security Assertion Markup Language (SAML) returned tooyour application by hello Azure Access Control Service (ACS).</span></span> <span data-ttu-id="5d8ef-105">hello útmutató épít, hello [hogyan tooAuthenticate webes felhasználók az Azure Access Control Service használatával Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) témakör, adja meg a kódot, amely hello SAML-információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-105">hello guide builds on hello [How tooAuthenticate Web Users with Azure Access Control Service Using Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) topic, by providing code that displays hello SAML information.</span></span> <span data-ttu-id="5d8ef-106">befejeződött hello alkalmazás hasonló toohello következő fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-106">hello completed application will look similar toohello following.</span></span>

![Példa SAML kimenet][saml_output]

<span data-ttu-id="5d8ef-108">Az ACS további információkért lásd: hello [további lépések](#next_steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-108">For more information on ACS, see hello [Next steps](#next_steps) section.</span></span>

> [!NOTE]
> <span data-ttu-id="5d8ef-109">hello Azure Access Services vezérlő szűrő egy közösségi technológiai előzetes.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-109">hello Azure Access Services Control Filter is a community technology preview.</span></span> <span data-ttu-id="5d8ef-110">Kiadás előtti szoftverként azt van hivatalosan Microsoft nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-110">As pre-release software, it is not formally supported by Microsoft.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="5d8ef-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5d8ef-111">Prerequisites</span></span>
<span data-ttu-id="5d8ef-112">Ebben az útmutatóban teljes toocomplete hello feladatok hello minta a [hogyan tooAuthenticate webes felhasználók az Azure Access Control Service használatával Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) és hello kiindulási pontjaként ehhez az oktatóanyaghoz használható.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-112">toocomplete hello tasks in this guide, complete hello sample at [How tooAuthenticate Web Users with Azure Access Control Service Using Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) and use it as hello starting point for this tutorial.</span></span>

## <a name="add-hello-jspwriter-library-tooyour-build-path-and-deployment-assembly"></a><span data-ttu-id="5d8ef-113">Hello JspWriter könyvtár tooyour build elérési útját és a központi telepítés szerelvény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5d8ef-113">Add hello JspWriter library tooyour build path and deployment assembly</span></span>
<span data-ttu-id="5d8ef-114">Hello tartalmazó hello könyvtár hozzáadása **javax.servlet.jsp.JspWriter** osztály tooyour szerelvény elérési útja és a központi telepítés létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-114">Add hello library that contains hello **javax.servlet.jsp.JspWriter** class tooyour build path and deployment assembly.</span></span> <span data-ttu-id="5d8ef-115">Tomcat használ, hogy van-e a hello könyvtár **jsp-api.jar**, hello Apache található **lib** mappát.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-115">If you are using Tomcat, hello library is **jsp-api.jar**, which is located in hello Apache **lib** folder.</span></span>

1. <span data-ttu-id="5d8ef-116">A Project Explorer Eclipse meg, kattintson a jobb gombbal **MyACSHelloWorld**, kattintson **Build elérési**, kattintson **Build elérési konfigurálása**, hello kattintson **szalagtárak** fülre, majd **külső JARs hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-116">In Eclipse's Project Explorer, right-click **MyACSHelloWorld**, click **Build Path**, click **Configure Build Path**, click hello **Libraries** tab, and then click **Add External JARs**.</span></span>
2. <span data-ttu-id="5d8ef-117">A hello **JAR kijelölés** párbeszédpanelen keresse meg a toohello szükséges JAR, válassza ki azt, és kattintson **nyissa meg**.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-117">In hello **JAR Selection** dialog, navigate toohello necessary JAR, select it, and then click **Open**.</span></span>
3. <span data-ttu-id="5d8ef-118">A hello **MyACSHelloWorld tulajdonságainak** párbeszédpanelen megnyitva, kattintson **telepítési szerelvény**.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-118">With hello **Properties for MyACSHelloWorld** dialog still open, click **Deployment Assembly**.</span></span>
4. <span data-ttu-id="5d8ef-119">A hello **webes telepítési szerelvény** párbeszédpanel, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-119">In hello **Web Deployment Assembly** dialog, click **Add**.</span></span>
5. <span data-ttu-id="5d8ef-120">A hello **új szerelvény irányelv** párbeszédpanel, kattintson a **Java Build elérési bejegyzések** , majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-120">In hello **New Assembly Directive** dialog, click **Java Build Path Entries** and then click **Next**.</span></span>
6. <span data-ttu-id="5d8ef-121">Válassza ki a megfelelő hello szalagtárat, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-121">Select hello appropriate library and click **Finish**.</span></span>
7. <span data-ttu-id="5d8ef-122">Kattintson a **OK** tooclose hello **MyACSHelloWorld tulajdonságainak** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-122">Click **OK** tooclose hello **Properties for MyACSHelloWorld** dialog.</span></span>

## <a name="modify-hello-jsp-file-toodisplay-saml"></a><span data-ttu-id="5d8ef-123">Hello JSP-fájl toodisplay SAML módosítása</span><span class="sxs-lookup"><span data-stu-id="5d8ef-123">Modify hello JSP file toodisplay SAML</span></span>
<span data-ttu-id="5d8ef-124">Módosítsa **index.jsp** toouse hello a következő kódot.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-124">Modify **index.jsp** toouse hello following code.</span></span>

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {

            try
            {
                String nodeName;
                int nChild, i;

                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");

                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }

                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                    {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    

                                 // If it is a text node, just print hello text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out hello child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                        for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);

                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate hello names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish hello sentence.
                                            out.print(".");
                                        }

                                     }
                                     out.println("<br>");

                                     // Process hello child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>

        <%
        try
        {
            String data  = (String) request.getAttribute("ACSSAML");

            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();

            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA);
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();

            // Iterate hello child nodes of hello doc.
            NodeList list = doc.getChildNodes();

            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e)
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }

        %>
    </body>
    </html>

## <a name="run-hello-application"></a><span data-ttu-id="5d8ef-125">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="5d8ef-125">Run hello application</span></span>
1. <span data-ttu-id="5d8ef-126">Futtassa az alkalmazást a hello számítógép emulátor vagy tooAzure leírt lépéseket hello segítségével telepítheti [hogyan tooAuthenticate webes felhasználók az Azure Access Control Service használatával Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="5d8ef-126">Run your application in hello computer emulator or deploy tooAzure, using hello steps documented at [How tooAuthenticate Web Users with Azure Access Control Service Using Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md).</span></span>
2. <span data-ttu-id="5d8ef-127">Elindít egy böngészőt, és nyissa meg a webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-127">Launch a browser and open your web application.</span></span> <span data-ttu-id="5d8ef-128">Tooyour alkalmazás bejelentkezést követően megjelenik a SAML információt, beleértve a hello hello identitásszolgáltató által biztosított biztonsági helyességi feltételt.</span><span class="sxs-lookup"><span data-stu-id="5d8ef-128">After you log on tooyour application, you'll see SAML information, including hello security assertion provided by hello identity provider.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d8ef-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5d8ef-129">Next steps</span></span>
<span data-ttu-id="5d8ef-130">toofurther ACS azon funkcióit és az összetettebb forgatókönyveket tooexperiment című [Access Control Service 2.0][Access Control Service 2.0].</span><span class="sxs-lookup"><span data-stu-id="5d8ef-130">toofurther explore ACS's functionality and tooexperiment with more sophisticated scenarios, see [Access Control Service 2.0][Access Control Service 2.0].</span></span>

[Prerequisites]: #pre
[Modify hello JSP file toodisplay SAML]: #modify_jsp
[Add hello JspWriter library tooyour build path and deployment assembly]: #add_library
[Run hello application]: #run_application
[Next steps]: #next_steps
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[How tooAuthenticate Web Users with Azure Access Control Service Using Eclipse]: active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
