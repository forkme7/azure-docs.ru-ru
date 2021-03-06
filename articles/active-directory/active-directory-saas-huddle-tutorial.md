---
title: "Учебник. Интеграция Azure Active Directory с Huddle | Документация Майкрософт"
description: "Узнайте, как настроить единый вход между Azure Active Directory и Huddle."
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 41f5d7df483e1cb0c982df983b16f4431b7ae8d8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a>Руководство. Интеграция Azure Active Directory с Huddle

В этом руководстве описано, как интегрировать Huddle с Azure Active Directory (Azure AD).

Интеграция Azure AD с приложением Huddle обеспечивает следующие преимущества:

- С помощью Azure AD вы можете контролировать доступ к Huddle.
- Вы можете включить автоматический вход пользователей в Huddle (единый вход) с учетной записью Azure AD.
- Вы можете управлять учетными записями централизованно — через портал Azure.

Подробнее узнать об интеграции приложений SaaS с Azure AD можно в разделе [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>предварительным требованиям

Чтобы настроить интеграцию Azure AD с Huddle, вам потребуется:

- подписка Azure AD;
- Подписка с поддержкой единого входа Huddle

> [!NOTE]
> Мы не рекомендуем использовать рабочую среду для проверки действий в этом учебнике.

При проверке действий в этом учебнике соблюдайте следующие рекомендации:

- Не используйте рабочую среду без необходимости.
- Если у вас нет пробной среды Azure AD, вы можете получить пробную версию на один месяц по [этой ссылке](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Описание сценария

В рамках этого руководства проводится проверка единого входа Azure AD в тестовой среде. Сценарий, описанный в этом учебнике, состоит из двух стандартных блоков.

1. Добавление Huddle из коллекции.
2. настройка и проверка единого входа в Azure AD.

## <a name="adding-huddle-from-the-gallery"></a>Добавление Huddle из коллекции
Чтобы настроить интеграцию Huddle с Azure AD, необходимо добавить Huddle из коллекции в список управляемых приложений SaaS.

**Чтобы добавить Huddle из коллекции, выполните следующие действия:**

1. На **[портале Azure](https://portal.azure.com)** в области навигации слева щелкните значок **Azure Active Directory**. 

    ![Active Directory][1]

2. Перейдите к разделу **Корпоративные приложения**. Затем выберите **Все приложения**.

    ![ПРИЛОЖЕНИЯ][2]
    
3. Чтобы добавить новое приложение, в верхней части диалогового окна нажмите кнопку **Создать приложение**.

    ![ПРИЛОЖЕНИЯ][3]

4. В поле поиска введите **Huddle**.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_search.png)

5. На панели результатов выберите **Huddle** и нажмите кнопку **Добавить**, чтобы добавить это приложение.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>настройка и проверка единого входа в Azure AD.

В этом разделе описана настройка и проверка единого входа Azure AD с Huddle с использованием тестового пользователя Britta Simon.

Чтобы единый вход работал, Azure AD необходимо знать, какой пользователь в Huddle соответствует пользователю в Azure AD. Иными словами, необходимо установить связь между пользователем Azure AD и соответствующим пользователем в Huddle.

Чтобы установить эту связь, назначьте **имя пользователя** в Azure AD в качестве значения **имени пользователя** в Huddle.

Чтобы настроить и проверить единый вход Azure AD в Huddle, вам потребуется выполнить действия в следующих стандартных блоках:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** необходима, чтобы пользователи могли использовать эту функцию.

2. **[Создание тестового пользователя Azure AD](#creating-an-azure-ad-test-user)** требуется для проверки работы единого входа в Azure AD от имени пользователя Britta Simon.

3. **[Создание тестового пользователя Huddle](#creating-a-huddle-test-user)** требуется для создания в Huddle пользователя Britta Simon, связанного с представлением этого же пользователя в Azure AD.

4. **[Назначение тестового пользователя Azure AD](#assigning-the-azure-ad-test-user)** необходимо, чтобы позволить Britta Simon использовать единый вход Azure AD;

5. **[Проверка единого входа](#testing-single-sign-on)** необходима, чтобы убедиться в корректной работе конфигурации.

### <a name="configuring-azure-ad-single-sign-on"></a>Настройка единого входа в Azure AD

В этом разделе описано, как включить единый вход Azure AD на портале Azure и настроить его в приложении Huddle.

**Чтобы настроить единый вход Azure AD в Huddle, выполните следующие действия:**

1. На портале Azure на странице интеграции с приложением **Huddle** щелкните **Единый вход**.

    ![Настройка единого входа][4]

2. В диалоговом окне **Единый вход** в разделе **Режим** выберите **Вход на основе SAML**, чтобы включить функцию единого входа.
 
    ![Настройка единого входа](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_samlbase.png)

3. В разделе **Домены и URL-адреса приложения Huddle** выполните следующие действия:

    ![Настройка единого входа](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_url.png)

    В текстовом поле **URL-адрес для входа** введите URL-адрес в следующем формате: `http://<company name>.huddle.com`

    > [!NOTE] 
    > Это значение приведено для справки. Вместо него необходимо указать фактический URL-адрес входа. Для получения данного значения обратитесь в [службу поддержки клиентов Huddle](https://huddle.zendesk.com). 

4. В разделе **Сертификат подписи SAML** щелкните **Сертификат (Base64)**, а затем сохраните файл сертификата на компьютере.

    ![Настройка единого входа](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_certificate.png) 

5. Нажмите кнопку **Сохранить** .

    ![Настройка единого входа](./media/active-directory-saas-huddle-tutorial/tutorial_general_400.png)

6. В разделе **Настройка Huddle** щелкните **Настроить Huddle**, чтобы открыть окно **Настройка единого входа**. Скопируйте **идентификатор сущности SAML и URL-адрес службы единого входа SAML** из раздела **Quick Reference** (Краткий справочник). 

    ![Настройка единого входа](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_configure.png) 
    
7. Чтобы настроить единый вход на стороне Huddle, вам необходимо отправить скачанный файл **сертификата**, **URL-адрес службы единого входа SAML** и **идентификатор сущности SAML** в [службу поддержки клиентов Huddle](https://huddle.zendesk.com). Специалисты службы поддержки настроят подключение единого входа SAML на обеих сторонах.  
   
    >[!NOTE]
    > Единый вход должна включить служба поддержки Huddle. Сразу же после завершения настройки вы получите уведомление. 
    > 

> [!TIP]
> Краткую версию этих инструкций теперь можно также прочитать на [портале Azure](https://portal.azure.com) во время настройки приложения.  После добавления этого приложения из раздела **Active Directory > Корпоративные приложения** просто выберите вкладку **Единый вход** и откройте встроенную документацию через раздел **Настройка** в нижней части страницы. Дополнительные сведения о встроенной документации см. в разделе [Встроенная документация Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985).
> 
   
### <a name="creating-an-azure-ad-test-user"></a>Создание тестового пользователя Azure AD

Цель этого раздела — создать на портале Azure тестового пользователя с именем Britta Simon.

![Создание пользователя Azure AD][100]

**Чтобы создать тестового пользователя в Azure AD, выполните следующие действия:**

1. На **портале Azure** в области навигации слева щелкните значок **Azure Active Directory**.

    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_01.png) 

2. Чтобы отобразить список пользователей, перейдите в раздел **Пользователи и группы** и щелкните **Все пользователи**.
    
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_02.png) 

3. Чтобы открыть диалоговое окно **Пользователь**, в верхней части диалогового окна щелкните **Добавить**.
 
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_03.png) 

4. На странице диалогового окна **Пользователь** выполните следующие действия.
 
    ![Создание тестового пользователя Azure AD](./media/active-directory-saas-huddle-tutorial/create_aaduser_04.png) 

    a. В текстовом поле **Имя** введите **BrittaSimon**.

    Б. В текстовом поле **Имя пользователя** введите **адрес электронной почты** учетной записи BrittaSimon.

    c. Выберите **Показать пароль** и запишите значение поля **Пароль**.

    d. Нажмите кнопку **Создать**.
 
### <a name="creating-a-huddle-test-user"></a>Создание тестового пользователя Huddle

Чтобы пользователи Azure AD могли выполнить вход в Huddle, они должны быть подготовлены для Huddle. В случае с Huddle подготовка выполняется вручную.

**Чтобы настроить подготовку учетных записей пользователей, выполните следующие действия.**

1. Выполните вход на веб-сайт компании **Huddle** в качестве администратора.
2. Нажмите **Рабочая область**.
3. Щелкните **People \> Invite People** (Пользователи > Пригласить пользователей).
   
   ![Люди](./media/active-directory-saas-huddle-tutorial/IC787838.png "Люди")

4. В разделе **Создание нового приглашения** выполните следующие действия.
   
   ![Создание приглашения](./media/active-directory-saas-huddle-tutorial/IC787839.png "Создание приглашения")
   
   a. В списке **Choose a team to invite people to join** (Выберите группу, в которую следует пригласить пользователей) выберите **группу**.

   Б. Введите **адрес электронной почты** действительной учетной записи Azure AD, которую вы хотите подготовить, в текстовом поле **Enter email address for people you'd like to invite** (Введите адреса электронной почты пользователей, которых вы хотите пригласить).

   c. Нажмите кнопку **Пригласить**.   
   
    >[!NOTE]
    > Владелец учетной записи Azure AD получит по электронной почте сообщение со ссылкой для активации учетной записи. 
    > 

>[!NOTE]
>Вы можете использовать любые другие средства создания учетной записи пользователя Huddle или API, предоставляемые Huddle, для подготовки учетных записей пользователя Azure AD. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Назначение тестового пользователя Azure AD

В этом разделе описано, как предоставить пользователю Britta Simon доступ к Huddle, чтобы он мог использовать единый вход Azure.

![Назначение пользователя][200] 

**Чтобы назначить пользователя Britta Simon в приложении Huddle, выполните следующие действия:**

1. На портале Azure откройте представление приложений, перейдите к представлению каталога, а затем выберите **Корпоративные приложения** и щелкните **Все приложения**.

    ![Назначение пользователя][201] 

2. В списке приложений выберите **Huddle**.

    ![Настройка единого входа](./media/active-directory-saas-huddle-tutorial/tutorial_huddle_app.png) 

3. В меню слева выберите **Пользователи и группы**.

    ![Назначение пользователя][202] 

4. Нажмите кнопку **Добавить**. Затем в диалоговом окне **Добавление назначения** выберите **Пользователи и группы**.

    ![Назначение пользователя][203]

5. В диалоговом окне **Пользователи и группы** в списке пользователей выберите **Britta Simon**.

6. В диалоговом окне **Пользователи и группы** нажмите кнопку **Выбрать**.

7. В диалоговом окне **Добавление назначения** нажмите кнопку **Назначить**.
    
### <a name="testing-single-sign-on"></a>Проверка единого входа

В этом разделе описано, как проверить конфигурацию единого входа Azure AD с помощью панели доступа.

Щелкнув элемент Huddle на панели доступа, вы автоматически войдете в приложение Huddle.
Дополнительные сведения о панели доступа см. в статье [Общие сведения о панели доступа](active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Список учебников по интеграции приложений SaaS с Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Что такое доступ к приложениям и единый вход с помощью Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_04.png
[100]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_100.png
[200]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-huddle-tutorial/tutorial_general_203.png
