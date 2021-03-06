---
title: Что такое доступ к приложениям и единый вход с помощью Azure Active Directory? | Документация Майкрософт
description: Используйте Azure Active Directory для включения единого входа для всех приложений SaaS и веб-приложений, необходимых для бизнеса.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.assetid: 75d1a3fd-b3c5-4495-a5c8-c4c24145ff00
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2017
ms.author: curtand
ms.reviewer: asmalser
ms.custom: it-pro
ms.openlocfilehash: f19d33c905d6153dffa1e7d5cdaea92ed1b94ff7
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/05/2018
---
# <a name="what-is-application-access-and-single-sign-on-with-azure-active-directory"></a>Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?
Благодаря единому входу пользователи получают доступ ко всем приложениям и ресурсам, необходимым для работы, выполнив только один вход с использованием одной учетной записи. После входа пользователю доступны все необходимые приложения без повторной проверки подлинности (например, ввода пароля).

Во многих организациях для эффективной работы пользователя используются такие приложения SaaS, как Office 365, Box и Salesforce. Обычно ИТ-специалистам необходимо создавать и обновлять отдельные учетные записи пользователей в каждом приложении SaaS, а пользователи должны запоминать пароль для каждого такого приложения.

Azure Active Directory переносит локальную службу Active Directory в облако. Поэтому с помощью основной учетной записи в организации пользователи могут входить не только на присоединенные к домену устройства и корпоративные ресурсы, но и во все необходимые для работы веб-приложения и приложения SaaS.

Таким образом, пользователям не нужно управлять несколькими наборами учетных данных. Доступ к приложениям предоставляется им или отменяется автоматически на основе членства в группах организации и статуса конкретного сотрудника. Azure Active Directory предоставляет элементы управления безопасностью и доступом, которые позволяют централизованно управлять доступом пользователей к приложениям SaaS.

Azure Active Directory обеспечивает простую интеграцию со многими современными популярными приложениями SaaS. Эта служба позволяет управлять удостоверениями и доступом, а также дает пользователям возможность выполнять единый вход непосредственно в приложения или находить их и запускать с портала, например из Office 365 или из панели доступа Azure AD.

Архитектура интеграции состоит из следующих четырех основных компонентов.

* Единый вход позволяет пользователям получать доступ к приложениям SaaS в соответствии с учетной записью организации в Azure AD. Именно единый вход дает пользователям возможность проходить проверку подлинности для входа в приложение с помощью одной учетной записи в организации.
* Подготовка пользователей позволяет выполнять подготовку пользователей и ее отмену в целевом приложении SaaS с учетом изменений, внесенных в Windows Server Active Directory и Azure AD. Подготовленная учетная запись дает возможность авторизоваться для использования приложения после проверки подлинности посредством единого входа.
* Централизованное управление доступом к приложениям на портале Azure предоставляет единую точку для доступа к приложениям SaaS и управления ими, а также дает возможность делегировать другим пользователям в организации принятие и утверждение решений о предоставлении доступа к приложениям.
* Единые средства создания отчетов о действиях пользователей в Azure AD и их мониторинга.

## <a name="how-does-single-sign-on-with-azure-active-directory-work"></a>Принцип работы единого входа с Azure Active Directory
Когда пользователь входит в приложение, он проходит проверку подлинности, в процессе которой необходимо доказать, что он является тем, за кого себя выдает. Без использования единого входа обычно это делается путем ввода пароля, который хранится в приложении. Пользователь должен знать этот пароль.

Azure AD поддерживает три разных способа входа в приложение.

* **Федеративный единый вход** позволяет приложениям выполнить перенаправление в Azure AD для аутентификации пользователя вместо запроса пароля. Этот способ поддерживается приложениями, совместимыми с такими протоколами, как SAML 2.0, WS-Federation или OpenID Connect, и является режимом единого входа с самыми широкими возможностями.
* **Единый вход на основе пароля** позволяет безопасно хранить пароль приложений и воспроизводить его с помощью расширения веб-браузера или мобильного приложения. Этот способ использует существующий процесс входа, предоставляемый приложением, однако позволяет администраторам управлять паролями, поэтому пользователю не нужно знать пароль.
* **Существующий единый вход** позволяет службе Azure AD использовать любой существующий способ единого входа, настроенный в приложении, и позволяет такому приложению связываться с порталом Office 365 или порталом панели доступа Azure AD, а также позволяет использовать дополнительные функции по созданию отчетов в Azure AD, если приложения запускаются там.

После аутентификации пользователя в приложении необходимо также подготовить учетную запись в приложении, которая указывает, где в приложении действуют разрешения и уровень доступа. Подготовка этой учетной записи может выполняться автоматически или администратор проводит ее вручную перед предоставлением пользователю единого доступа.

 Дополнительные сведения об этих режимах единого входа и подготовки см. ниже.

### <a name="federated-single-sign-on"></a>Федеративный единый вход
Федеративный единый вход позволяет пользователям вашей организации автоматически входить в сторонние приложения SaaS с помощью Azure AD, используя данные учетной записи пользователя из Azure AD.

В этом случае, когда уже выполнен вход в Azure AD и требуется получить доступ к ресурсам, которые управляются сторонним приложением SaaS, федерация позволяет пользователю не проходить повторную аутентификацию.

Azure AD поддерживает федеративный единый вход в приложения с поддержкой протоколов SAML 2.0, WS-Federation или OpenID Connect.

См. также статью [Управление сертификатами для федеративного единого входа в Azure Active Directory](active-directory-sso-certs.md).

### <a name="password-based-single-sign-on"></a>Единый вход на основе пароля
Настройка единого входа на основе пароля позволяет пользователям вашей организации автоматически входить в сторонние приложения SaaS при помощи Azure AD, используя данные учетной записи пользователя из стороннего приложения SaaS. При включении данной функции служба Azure AD собирает и безопасно хранит данные учетной записи пользователя и связанный с ней пароль.

Azure AD поддерживает единый вход на основе пароля для любого облачного приложения со страницей входа на базе HTML. Используя настраиваемый подключаемый модуль браузера, служба Azure AD автоматизирует вход пользователя в систему. Для этого она безопасно получает учетные данные приложений (имя пользователя и пароль) из каталога и вводит их на странице входа в приложение от имени пользователя. Существует два варианта использования.

1. **Администратор управляет учетными данными.** Администраторы могут создавать учетные данные приложений и управлять ими и назначать эти учетные данные пользователям или группам, которым необходим доступ к приложению. В этих случаях пользователям не нужно знать учетные данные, а доступ к приложению они получают с помощью единого входа: они просто выбирают его на панели доступа или переходят по предоставленной ссылке. Это позволяет администраторам управлять жизненным циклом учетных данных, а конечным пользователям не нужно запоминать пароли конкретного приложения или управлять ими. При автоматическом входе учетные данные скрыты от пользователя. Однако технически их можно открыть с помощью инструментов веб-отладки. Пользователи и администраторы должны использовать те же политики безопасности, что и в случае, когда учетные данные вводятся непосредственно пользователем. Учетные данные, предоставляемые администратором, удобно использовать, когда доступ к учетной записи предоставляется многим пользователям, например для социальных сетей или для приложений с общим доступом к документам.
2. **Пользователь управляет учетными данными.** Администраторы могут назначать приложения конечным пользователям или группам и разрешать пользователям вводить свои учетные данные непосредственно во время доступа к приложению в первый раз на панели доступа. Это удобно для конечных пользователей, т. к. им не требуется постоянно вводить пароли для конкретных приложений во время каждого доступа к ним. Пользователи могут и дальше управлять своими паролями, при необходимости обновляя или удаляя их. Этот вариант можно также использовать как средство для административного управления учетными данными, когда администратор может в будущем настроить новые учетные данные для приложения, не изменяя параметры доступа к нему для конечного пользователя.

В обоих случаях учетные данные хранятся в зашифрованном виде в каталоге и передаются только по протоколу HTTPS во время автоматического входа. С помощью единого входа на основе пароля служба Azure AD предлагает удобное решение по управлению доступом к удостоверениям для приложений, которые не поддерживают протоколы федерации.

Единый вход на основе пароля использует расширение браузера для безопасного получения данных о приложениях и пользователях из службы Azure AD и применения их к службе. Большинство сторонних приложений SaaS, поддерживаемых Azure AD, поддерживает эту возможность.

Единый вход с использованием пароля работает в таких браузерах:
* Internet Explorer 8, 9, 10, 11 (в Windows 7 или более поздней версии);
* Edge в Windows 10 Anniversary Edition или более поздней версии; 
* Chrome (начиная с Windows 7 и Mac OS X);
* Firefox 26.0 и более поздние версии (начиная с Windows XP с пакетом обновления 2 (SP2) и Mac OS X 10.6).

### <a name="existing-single-sign-on"></a>Существующий единый вход
При настройке единого входа для приложения на портале Azure доступен третий параметр: "Существующий единый вход". Этот параметр позволяет администраторам создавать ссылку на приложение и помещать ее на панель доступа для выбранных пользователей.

Например, если существует приложение, которое выполняет проверку подлинности пользователей с помощью служб федерации Active Directory 2.0, администратор может использовать параметр «Существующий единый вход» для создания ссылки на него на панели доступа. Когда пользователи переходят по ссылке, проверка подлинности выполняется с помощью служб федерации Active Directory 2.0 или другого решения существующего единого входа, предоставляемого приложением.

### <a name="user-provisioning"></a>Подготовка пользователей
Для определенных приложений служба Azure AD обеспечивает автоматическую подготовку пользователей и отмену подготовки учетных записей в сторонних приложениях SaaS на портале управления Azure, используя сведения удостоверений Windows Server Active Directory или Azure AD. Когда пользователь получает разрешения в Azure AD для одного из этих приложений, учетная запись может быть создана (подготовлена) автоматически в целевом приложении SaaS.

Если пользователь удален или его данные изменены в Azure AD, эти изменения также отражаются в приложении SaaS. Это означает, что настройка автоматического управления жизненным циклом удостоверений позволяет администраторам выполнять автоматическую подготовку (и отмену подготовки) из приложений SaaS и управлять ею. В Azure AD такая автоматизация управления жизненным циклом удостоверений обеспечивается с помощью подготовки пользователей.

Дополнительные сведения см. в статье [Автоматическая подготовка пользователей и ее отзыв для приложений SaaS в Azure Active Directory](active-directory-saas-app-provisioning.md).

## <a name="get-started-with-the-azure-ad-application-gallery"></a>Начало работы с коллекцией приложений Azure AD
Готовы начать работу? Для развертывания единого входа между приложениями Azure AD и SaaS, используемых в вашей организации, следуйте приведенным ниже рекомендациям.

### <a name="using-the-azure-ad-application-gallery"></a>Использование коллекции приложений Azure AD
[Коллекция приложений Azure Active Directory](https://azure.microsoft.com/marketplace/active-directory/all/) содержит список приложений, которые поддерживают единый вход с помощью Azure Active Directory.

![Коллекция приложений Azure в Интернете](media/active-directory-appssoaccess-whatis/onlineappgallery.png)

Ниже приведены советы для поиска приложений в соответствии с поддерживаемыми функциями.

* Azure AD поддерживает автоматическую подготовку и ее отзыв для всех приложений категории «Основные» в [коллекции приложений Azure Active Directory](https://azure.microsoft.com/marketplace/active-directory/all/).
* Список федеративных приложений, поддерживающих федеративный единый вход с помощью таких протоколов, как SAML, WS-Federation или OpenID Connect, см. [здесь](http://social.technet.microsoft.com/wiki/contents/articles/20235.azure-active-directory-application-gallery-federated-saas-apps.aspx).

Найдите необходимое приложение и следуйте пошаговым инструкциям, указанным в коллекции приложений и на портале управления Azure, чтобы включить единый вход.

### <a name="application-not-in-the-gallery"></a>Приложение отсутствует в коллекции?
Если необходимое приложение не найдено в коллекции приложений Azure AD, доступны следующие варианты.

* **Добавление отсутствующего приложения.** С помощью категории "Пользовательские" в коллекции приложений на портале управления Azure можно подключить отсутствующее в списке приложение, которое использует ваша организация. Можно добавить любое приложение, поддерживающее протокол SAML 2.0 в качестве федеративного приложения, или любое приложения со страницей для входа на базе HTML в качестве приложения с паролем для единого входа. Дополнительные сведения см. в статье о [добавлении собственных приложений](application-config-sso-how-to-configure-federated-sso-non-gallery.md).
* **Добавление самостоятельно разработанного приложения.** Если вы самостоятельно разработали приложение, то с помощью рекомендаций в документации разработчика для Azure AD можно реализовать федеративный единый вход или выполнить подготовку с использованием API Graph Azure AD. Для получения дополнительных сведений см. следующие ресурсы.
  
  * [Сценарии аутентификации в Azure Active Directory](active-directory-authentication-scenarios.md)
  * [https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet](https://github.com/AzureADSamples/WebApp-WebAPI-MultiTenant-OpenIdConnect-DotNet)
  * [https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore](https://github.com/AzureADSamples/NativeClient-WebAPI-MultiTenant-WindowsStore)
* **Запросить интеграцию приложения.** Вы можете запросить поддержку для необходимого приложения на [форуме отзывов Azure AD](https://feedback.azure.com/forums/169401-azure-active-directory/).

### <a name="using-the-azure-portal"></a>Использование портала Azure
Для настройки единого входа в приложение можно использовать расширение Active Directory на портале Azure. Для начала необходимо выбрать каталог из раздела Active Directory на портале:

![][2]

Для управления сторонними приложениями SaaS перейдите на вкладку «Приложения» выбранного каталога. В этом представлении администраторы могут выполнять следующие действия:

* добавлять новые приложения из коллекции Azure AD и самостоятельно разработанные приложения;
* удалять интегрированные приложения;
* управлять уже интегрированными приложениями.

К стандартным задачам администрирования для стороннего приложения SaaS относятся следующие:

* включение единого входа с помощью Azure AD, используя пароль единого входа или федеративный единый вход (если это доступно для целевого приложения SaaS);
* подготовка пользователей и ее отмена (при необходимо);
* выбор пользователей для предоставления им доступа к приложениям с включенной подготовкой пользователей.

Для конфигурации приложений из коллекции, поддерживающих федеративный единый вход, обычно необходимо указать дополнительные параметры конфигурации, такие как сертификаты и метаданные для создания федеративных отношений доверия между сторонним приложением и Azure AD. Мастер настройки поможет выполнить все настройки и обеспечит простой доступ к данным и инструкциям приложения SaaS.

Для приложений из коллекции, поддерживающих автоматическую подготовку пользователей, необходимо предоставить службе Azure AD разрешения для управления учетными записями в приложении SaaS. Как минимум, необходимо указать учетные данные, которые службе Azure AD следует использовать для проверки подлинности в целевом приложении. Необходимость указывать дополнительные параметры конфигурации зависит от требований приложения.

## <a name="deploying-azure-ad-integrated-applications-to-users"></a>Развертывание интегрированных приложений Azure AD для пользователей
Azure AD предоставляет несколько настраиваемых способов развертывания приложений для пользователей в организации.

* панель доступа Azure AD;
* средство запуска приложений Office 365;
* Прямой вход в федеративные приложения
* прямые ссылки на федеративные приложения, приложения на основе пароля или существующие приложения;

Вы сами выбираете метод (или методы) развертывания в своей организации.

### <a name="azure-ad-access-panel"></a>панель доступа Azure AD;
Панель доступа по адресу https://myapps.microsoft.com представляет собой веб-портал, который позволяет пользователям с учетной записью организации в Azure Active Directory просматривать и запускать облачные приложения, к которым администратор Azure AD предоставил доступ. Если вы являетесь пользователем [Azure Active Directory Premium](https://azure.microsoft.com/pricing/details/active-directory/), вы также можете использовать возможности панели доступа для самостоятельного управления группами.

![Панель доступа Azure AD](media/active-directory-appssoaccess-whatis/azure-ad-access-panel.png)

Панель доступа отделена от портала Azure, для нее не требуется подписка Azure или Office 365.

Дополнительные сведения о панели доступа Azure AD см. в статье с [общими сведениями о панели доступа](active-directory-saas-access-panel-introduction.md).

### <a name="office-365-application-launcher"></a>средство запуска приложений Office 365;
В организациях, в которых выполнено развертывание Office 365, приложения, назначенные пользователям через Azure AD, также отображаются на портале Office 365 по адресу https://portal.office.com/myapps. Благодаря этому пользователи в организации могут легко запускать приложения без входа на второй портал. Это решение рекомендуется для запуска приложений в организациях, использующих Office 365.

![][4]

Дополнительные сведения о средстве запуска приложений Office 365 см. в статье [Добавление приложений в средство запуска приложений Office 365](https://msdn.microsoft.com/office/office365/howto/connect-your-app-to-o365-app-launcher).

### <a name="direct-sign-on-to-federated-apps"></a>Прямой вход в федеративные приложения
Большинство федеративных приложений, поддерживающих SAML 2.0, WS-Federation или OpenID Connect, также дают пользователям возможность запустить приложение, а затем выполнить вход через Azure AD с помощью автоматического перенаправления или ссылки для входа. Это называется входом, инициированным поставщиком услуг. Большинство федеративных приложений в коллекции приложений Azure AD поддерживает его (дополнительные сведения см. в документах, доступных по ссылкам в мастере настройки единого входа в приложении на портале управления Azure).

![][5]

### <a name="direct-sign-on-links-for-federated-password-based-or-existing-apps"></a>Ссылки для прямого входа в федеративные приложения, приложения с паролем или существующие приложения
Azure AD также поддерживает ссылки для прямого единого входа в отдельные приложения, которые поддерживают единый вход на основе пароля, существующий единый вход или любой вид федеративного единого входа.

Эти ссылки представляют собой специально созданные URL-адреса, позволяющие пользователю войти в конкретное приложение с помощью Azure AD, не запуская его из панели доступа Azure AD или Office 365. Эти URL-адреса единого входа можно найти на вкладке «Панель мониторинга» любого предварительно интегрированного приложения в разделе Active Directory на портале управления Azure, как показано на следующем снимке экрана.

![][6]

Такие ссылки можно скопировать и вставить везде, где нужно указать ссылку для входа в выбранное приложение. Это может быть сообщение электронной почты или веб-портал, настроенный для доступа пользователей к приложениям. Вот пример URL-адреса для прямого единого входа Azure AD для Twitter:

`https://myapps.microsoft.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Как и в случае с URL-адресами для панели доступа конкретной организации, этот URL-адрес можно изменить, добавив один из активных или проверенных доменов для каталога после имени домена myapps.microsoft.com. Это позволяет загрузить фирменную символику организации непосредственно на странице входа до того, как пользователь введет свой идентификатор. Например:

`https://myapps.microsoft.com/contosobuild.com/signin/Twitter/230848d52c8745d4b05a60d29a40fced`

Когда авторизованный пользователь щелкнет одну из этих ссылок для конкретного приложения, сначала отобразится страница входа организации (если он еще не выполнил вход), а после входа в систему произойдет перенаправление в приложение без отображения панели доступа. Если пользователь не выполнил необходимые условия для доступа к приложению, например отсутствует расширение браузера для единого входа на основе пароля, отобразится запрос на установку отсутствующего расширения. URL-адрес ссылки также остается неизменным при изменении конфигурации единого входа для приложения.

Эти ссылки используют тот же механизм управления доступом, что и панель доступа и Office 365, и только пользователи или группы, которым назначено приложение на портале управления Azure, смогут пройти проверку подлинности. Неавторизированный пользователь увидит сообщение о том, что ему не предоставлен доступ. Кроме того, отобразится ссылка для загрузки панели доступа, на которой пользователь может увидеть приложения, к которым у него есть доступ.

## <a name="related-articles"></a>Связанные статьи
* [Указатель статьей по управлению приложениями в Azure Active Directory](active-directory-apps-index.md)
* [Список учебников по интеграции приложений SaaS с Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Поиск несанкционированных облачных приложений с Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md)
* [Введение в управление доступом к приложениям](active-directory-managing-access-to-apps.md)
* [Сравнение возможностей управления внешними удостоверениями в Azure AD](active-directory-b2b-compare-external-identities.md)

<!--Image references-->
[1]: ./media/active-directory-appssoaccess-whatis/onlineappgallery.png
[2]: ./media/active-directory-appssoaccess-whatis/azuremgmtportal.png
[3]: ./media/active-directory-appssoaccess-whatis/accesspanel.png
[4]: ./media/active-directory-appssoaccess-whatis/officeapphub.png
[5]: ./media/active-directory-appssoaccess-whatis/workdaymobile.png
[6]: ./media/active-directory-appssoaccess-whatis/deeplink.png
