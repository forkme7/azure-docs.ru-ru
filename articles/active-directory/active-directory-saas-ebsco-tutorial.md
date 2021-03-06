---
title: "Руководство по интеграции Azure Active Directory с приложением EBSCO | Документация Майкрософт"
description: "Узнайте, как настроить единый вход между Azure Active Directory и EBSCO."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 144f7f65-69e9-4016-a151-fe1104fd6ba8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: jeedes
ms.openlocfilehash: ea7fe09c31d88cf2095b3a3777b6b1f9feb8df46
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/09/2018
---
# <a name="tutorial-azure-active-directory-integration-with-ebsco"></a>Руководство по интеграции Azure Active Directory с приложением EBSCO

В этом руководстве описано, как интегрировать EBSCO с Azure Active Directory (Azure AD).

Интеграция Azure AD с EBSCO обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к EBSCO.
- Вы можете включить автоматический вход пользователей в EBSCO (единый вход) с использованием учетной записи Azure AD.
- Вы можете управлять учетными записями централизованно — на портале Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>предварительным требованиям

Чтобы настроить интеграцию Azure AD с EBSCO, вам потребуется:

- подписка Azure AD;
- подписка на EBSCO с поддержкой единого входа.

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете [получить пробную версию на один месяц](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария
В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление EBSCO из коллекции
2. настройка и проверка единого входа в Azure AD.

## <a name="adding-ebsco-from-the-gallery"></a>Добавление EBSCO из коллекции
Чтобы настроить интеграцию EBSCO с Azure AD, необходимо добавить EBSCO из коллекции в список управляемых приложений SaaS.

**Чтобы добавить EBSCO из коллекции, сделайте следующее:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Кнопка "Azure Active Directory"][1]

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![Колонка "Корпоративные приложения"][2]
    
3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![Кнопка "Новое приложение"][3]

4. В поле поиска введите **EBSCO**, выберите **EBSCO** на панели результатов и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![EBSCO в списке результатов](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Настройка и проверка единого входа в Azure AD

В этом разделе описана настройка и проверка единого входа Azure AD в EBSCO с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходимо знать, какой пользователь в EBSCO соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в EBSCO.

Чтобы настроить и проверить единый вход Azure AD в EBSCO, вам потребуется выполнить действия в следующих стандартных блоках.

1. **[Настройка единого входа Azure AD](#configure-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.
2. **[Создание тестового пользователя Azure AD](#create-an-azure-ad-test-user)** требуется для проверки работы единого входа Azure AD от имени пользователя Britta Simon.
3. **[Создание тестового пользователя EBSCO](#create-an-ebsco-test-user)** — можно автоматизировать подготовку и персонализацию пользователя в EBSCOhost. Приложение EBSCO поддерживает JIT-подготовку пользователей.
4. **[Назначение тестового пользователя Azure AD](#assign-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD.
5. **[Проверка единого входа](#test-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configure-azure-ad-single-sign-on"></a>Настройка единого входа Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении EBSCO.

**Чтобы настроить единый вход Azure AD в EBSCO, выполните следующие действия.**

1. На портале Azure на странице интеграции с приложением **EBSCO** щелкните **Единый вход**.

    ![Ссылка "Настройка единого входа"][4]

2. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Диалоговое окно "Единый вход"](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_samlbase.png)

3. Если вы хотите настроить приложение в режиме, инициированном **поставщиком удостоверений**, то в разделе **Домены и URL-адреса приложения EBSCO** выполните следующие действия:

    ![Сведения о домене и URL-адресах единого входа для приложения EBSCO](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_url.png)

    В текстовом поле **Идентификатор** введите URL-адрес: `pingsso.ebscohost.com`

4. Установите флажок **Показать дополнительные параметры URL-адресов**, и выполните следующее действие, если хотите настроить приложение для работы в режиме, инициируемом **поставщиком услуг**:

    ![Сведения о домене и URL-адресах единого входа для приложения EBSCO](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_url1.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `http://search.ebscohost.com/login.aspx?authtype=sso&custid=<unique EBSCO customer ID>&profile=<profile ID>`
     
    > [!NOTE] 
    > Значение URL-адреса входа приведено для примера. Вместо него необходимо указать фактический URL-адрес входа. Чтобы получить это значение, обратитесь в [службу поддержки клиентов EBSCO](mailto:sso@ebsco.com). 

    o   **Уникальные элементы:**  

    o   **custid** — введите уникальный идентификатор клиента EBSCO. 

    o   **profile** — клиенты могут настроить ссылку, чтобы направлять пользователей к определенному профилю (в зависимости от приобретенных продуктов в EBSCO). Они могут ввести определенный идентификатор профиля. Основными идентификаторами являются eds (служба обнаружения EBSCO) и ehost (базы данных EBSOCOhost). Такие же инструкции приведены [здесь](https://help.ebsco.com/interfaces/EBSCOhost/EBSCOhost_FAQs/How_do_I_set_up_direct_links_to_EBSCOhost_profiles_and_or_databases#profile).

5. Приложение EBSCO ожидает утверждения SAML в определенном формате. Настройте следующие утверждения для этого приложения. Управлять значениями этих атрибутов можно в разделе **Атрибуты пользователя** на странице интеграции приложения. На следующем снимке экрана приведен пример.
    
    ![Настройка единого входа](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_attribute.png)

    > [!Note]
    > Атрибут **имени** является обязательным и сопоставляется с **идентификатором пользователя** в приложении EBSCO. Он добавлен по умолчанию, поэтому его не нужно добавлять вручную.
    
6. В разделе **Атрибуты пользователя** диалогового окна **Единый вход** настройте атрибут токена SAML, как показано на рисунке выше, и выполните следующие действия:
    
    | Имя атрибута | Значение атрибута |
    | ---------------| --------------- |    
    | FirstName   | user.givenname |
    | LastName   | user.surname |
    | Email   | user.mail |

    a. Щелкните **Добавить атрибут**, чтобы открыть диалоговое окно **Добавление атрибута**.

    ![Настройка единого входа](./media/active-directory-saas-ebsco-tutorial/tutorial_officespace_04.png)

    ![Настройка единого входа](./media/active-directory-saas-ebsco-tutorial/tutorial_attribute_05.png)
    
    Б. В текстовом поле **Имя** введите имя атрибута, отображаемое для этой строки.
    
    c. В списке **Значение** выберите значение атрибута, отображаемое для этой строки.
    
    d. Нажмите кнопку **ОК**.

7. В разделе **Сертификат подписи SAML** щелкните **Metadata XML** (Метаданные XML) и сохраните файл метаданных на компьютере.

    ![Ссылка для скачивания сертификата](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_certificate.png) 

8. Нажмите кнопку **Сохранить** .

    ![Кнопка "Сохранить" в окне настройки единого входа](./media/active-directory-saas-ebsco-tutorial/tutorial_general_400.png)
    
9. Чтобы настроить единый вход на стороне **EBSCO**, отправьте скачанный **XML-файл метаданных** в [службу поддержки EBSCO](mailto:sso@ebsco.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).

### <a name="create-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

   ![Создание тестового пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На портале Azure в области слева нажмите кнопку **Azure Active Directory**.

    ![Кнопка "Azure Active Directory"](./media/active-directory-saas-ebsco-tutorial/create_aaduser_01.png)

2. Чтобы открыть список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.

    ![Ссылки "Пользователи и группы" и "Все пользователи"](./media/active-directory-saas-ebsco-tutorial/create_aaduser_02.png)

3. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна **Все пользователи** щелкните **Добавить**.

    ![Кнопка "Добавить"](./media/active-directory-saas-ebsco-tutorial/create_aaduser_03.png)

4. В диалоговом окне **Пользователь** сделайте следующее.

    ![Диалоговое окно "Пользователь"](./media/active-directory-saas-ebsco-tutorial/create_aaduser_04.png)

    a. В поле **Имя** введите **BrittaSimon**.

    Б. В поле **Имя пользователя** введите адрес электронной почты для пользователя Britta Simon.

    c. Установите флажок **Показать пароль** и запишите значение, которое отображается в поле **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="create-an-ebsco-test-user"></a>Создание тестового пользователя EBSCO

В EBSCO подготовка пользователей выполняется автоматически.

**Чтобы подготовить учетную запись пользователя, сделайте следующее:**

Azure AD передает в приложение EBSCO необходимые данные. Подготовка пользователей EBSCO может выполняться автоматически или на основе разовой формы. Это зависит от того, имеется ли у клиента много учетных записей EBSCOhost с сохраненными пользовательскими параметрами. К тому же, это можно выяснить в [службе поддержки EBSCO](mailto:sso@ebsco.com) во время реализации сценария. В любом случае клиент может не создавать учетные записи EBSCOhost до тестирования.

   >[!Note]
   >Подготовку или персонализацию пользователя EBSCOhost можно автоматизировать. Обратитесь в [службу поддержки EBSCO](mailto:sso@ebsco.com), чтобы получить сведения о JIT-подготовке пользователей. 
 
### <a name="assign-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как предоставить пользователю Britta Simon доступ к EBSCO, чтобы он мог использовать единый вход Azure.

![Назначение роли пользователя][200] 

**Чтобы назначить пользователя Britta Simon в EBSCO, сделайте следующее:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

2. В списке приложений выберите **EBSCO**.

    ![Ссылка на EBSCO в списке приложений](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_app.png)  

3. В меню слева выберите **Пользователи и группы**.

    ![Ссылка "Пользователи и группы"][202]

4. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Область "Добавление назначения"][203]

5. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

6. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="test-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

1. Щелкнув плитку EBSCO на панели доступа, вы автоматически войдете в приложение EBSCO.
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).

2. Когда вы перейдете в приложение, щелкните **Sign in** (Войти) в правом верхнем углу.

    ![Вход в EBSCO в списке приложений](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_signin.png)
 
3. Вам будет единожды предложено связать учетную запись организации (SAML), выбрав один из двух вариантов: **Link your existing MyEBSCOhost account to your institution account now** (Связать имеющуюся учетную запись MyEBSCOhost с учетной записью организации сейчас) или **Create a new MyEBSCOhost account and link it to your institution account** (Создать учетную запись MyEBSCOhost и связать ее с учетной записью организации). Учетная запись используется для персонализации в приложении EBSCOhost. Выберите вариант **Create a new account** (Создать учетную запись), и вы увидите, что форма персонализации предварительно заполнена значениями из ответа SAML, как показано на снимке экрана ниже. Нажмите кнопку **Continue** (Продолжить), чтобы сохранить выбранные значения.
    
     ![Пользователь EBSCO в списке приложений](./media/active-directory-saas-ebsco-tutorial/tutorial_ebsco_user.png)

4. Выполнив указанные действия по настройке, очистите файлы cookie и кэш и повторно выполните вход. Вам не нужно выполнять вход вручную еще раз, так как параметры персонализации сохранены.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ebsco-tutorial/tutorial_general_203.png

