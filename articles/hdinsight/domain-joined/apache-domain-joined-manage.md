---
title: Управление присоединенными к домену кластерами HDInsight в Azure
description: Узнайте, как управлять присоединенными к домену кластерами HDInsight
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: omidm
ms.openlocfilehash: 9875d9884f04d26ebfbd44e858beb272c2306958
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/19/2018
---
# <a name="manage-domain-joined-hdinsight-clusters"></a>Управление присоединенными к домену кластерами HDInsight
Узнайте, что такое пользователи и роли в кластерах HDInsight, присоединенных к домену, и как управлять присоединенными к домену кластерами HDInsight.

## <a name="use-vscode-to-link-to-domain-joined-cluster"></a>Использование Visual Studio Code для связывания присоединенного к домену кластера

Вы можете связать обычный кластер с помощью управляемого имени пользователя Ambari, а кластер безопасности — с помощью имени пользователя домена (например, user1@contoso.com).
1. Откройте палитру команд, нажав **CTRL+SHIFT+P** и введите **HDInsight: Link a cluster** (HDInsight: связать кластер).

   ![команда связывания кластера](./media/apache-domain-joined-manage/link-cluster-command.png)

2. Введите URL-адрес кластера HDInsight, имя пользователя и пароль и выберите тип кластера. Если проверка пройдена, отобразится сообщение об успешном выполнении.
   
   ![диалоговое окно связывания кластера](./media/apache-domain-joined-manage/link-cluster-process.png)

   > [!NOTE]
   > Если кластер зарегистрирован в подписке Azure и связан, используется имя пользователя и пароль для связывания. 
   
3. Данные связанного кластера можно просмотреть с помощью команды **перечисления кластеров**. Теперь в этот связанный кластер можно отправить скрипт.

   ![связанный кластер](./media/apache-domain-joined-manage/linked-cluster.png)

4. Также можно удалить связь кластера. Для этого введите **HDInsight: Unlink a cluster** (HDInsight: удалить связь кластера) в палитре команд.

## <a name="use-intellij-to-link-to-domain-joined-cluster"></a>Использование IntelliJ для связывания присоединенного к домену кластера

Вы можете связать обычный кластер с помощью управляемого имени пользователя Ambari, а кластер безопасности — с помощью имени пользователя домена (например, user1@contoso.com). 
1. Щелкните **Link a cluster** (Связывание кластера) в **обозревателе Azure**.

   ![контекстное меню связывания кластера](./media/apache-domain-joined-manage/link-a-cluster-context-menu.png)

2. Введите **имя кластера**, **имя пользователя** и **пароль**. В случае сбоя аутентификации проверьте имя пользователя и пароль. При необходимости добавьте учетную запись хранения, ключ хранилища, затем выберите контейнер из контейнера хранилища. Информация о хранилище предназначена для обозревателя хранилищ, который находится в дереве слева
   
   ![диалоговое окно связывания кластера](./media/apache-domain-joined-manage/link-a-cluster-dialog.png)

   > [!NOTE]
   > Если кластер зарегистрирован в подписке Azure и связан, используется ключ к хранилищу данных, имя пользователя и пароль для связывания.
   > ![обозреватель хранилищ в IntelliJ](./media/apache-domain-joined-manage/storage-explorer-in-IntelliJ.png)

   
3. Если данные введены правильно, связанный кластер отобразится в узле **HDInsight**. Теперь в этот связанный кластер можно отправить приложение.

   ![связанный кластер](./media/apache-domain-joined-manage/linked-cluster-intellij.png)

4. В **обозревателе Azure** также можно удалить связь кластера.
   
   ![кластер с удаленной связью](./media/apache-domain-joined-manage/unlink.png)

## <a name="use-eclipse-to-link-to-domain-joined-cluster"></a>Использование Eclipse для связывания присоединенного к домену кластера

Вы можете связать обычный кластер с помощью управляемого имени пользователя Ambari, а кластер безопасности — с помощью имени пользователя домена (например, user1@contoso.com).
1. Щелкните **Link a cluster** (Связывание кластера) в **обозревателе Azure**.

   ![контекстное меню связывания кластера](./media/apache-domain-joined-manage/link-a-cluster-context-menu.png)

2. Введите **имя кластера**, **имя пользователя** и **пароль**, затем нажмите кнопку "ОК", чтобы связать кластер. При необходимости введите учетную запись хранения, ключ к хранилищу данных, а затем выберите в представлении в виде дерева контейнер хранилища для обозревателя хранилищ.
   
   ![диалоговое окно связывания кластера](./media/apache-domain-joined-manage/link-cluster-dialog.png)
   
   > [!NOTE]
   > Если кластер зарегистрирован в подписке Azure и связан, используется ключ к хранилищу данных, имя пользователя и пароль для связывания.
   > ![обозреватель хранилищ в Eclipse](./media/apache-domain-joined-manage/storage-explorer-in-Eclipse.png)

3. Если данные введены правильно, связанный кластер отобразится в узле **HDInsight** после нажатия кнопки "ОК". Теперь в этот связанный кластер можно отправить приложение.

   ![связанный кластер](./media/apache-domain-joined-manage/linked-cluster-intellij.png)

4. В **обозревателе Azure** также можно удалить связь кластера.
   
   ![кластер с удаленной связью](./media/apache-domain-joined-manage/unlink.png)

## <a name="access-the-clusters-with-enterprise-security-package"></a>Получите доступ к кластерам с помощью пакета безопасности предприятия.

Пакет безопасности предприятия (прежнее название — HDInsight Premium) обеспечивает многопользовательский доступ к кластеру, где выполняется аутентификация Active Directory, а также авторизация с использованием Apache Ranger и ACL ADLS. Авторизация обеспечивает безопасное разделение пользователей, разрешая только привилегированным лицам доступ к данным на основе политик авторизации.

Обеспечение безопасности и изоляция пользователей важны для кластера HDInsight с пакетом безопасности предприятия. Для выполнения этих требований SSH-доступ к кластеру с пакетом безопасности предприятия блокируется. В следующей таблице описаны способы доступа, рекомендуемые для каждого типа кластера:

|Рабочая нагрузка|Сценарий|Способ доступа к данным|
|--------|--------|-------------|
|Hadoop|Hive — интерактивные задания и запросы |<ul><li>[Beeline](#beeline)</li><li>[Представление Hive](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Инструменты Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Spark|Интерактивные задания и запросы, интерактивные задания PySpark|<ul><li>[Beeline](#beeline)</li><li>[Zeppelin с Livy](../spark/apache-spark-zeppelin-notebook.md)</li><li>[Представление Hive](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Инструменты Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Spark|Сценарии пакетной службы — отправка Spark, PySpark|<ul><li>[Livy](../spark/apache-spark-livy-rest-interface.md)</li></ul>|
|Интерактивный запрос (LLAP)|Interactive|<ul><li>[Beeline](#beeline)</li><li>[Представление Hive](../hadoop/apache-hadoop-use-hive-ambari-view.md)</li><li>[ODBC/JDBC – Power BI](../hadoop/apache-hadoop-connect-hive-power-bi.md)</li><li>[Инструменты Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)</li></ul>|
|Любой|Установка пользовательского приложения|<ul><li>[Действия сценариев](../hdinsight-hadoop-customize-cluster-linux.md)</li></ul>|


Использование стандартных API помогает обеспечить безопасность. Кроме того, вы получаете такие преимущества:

1.  **Управление** — вы можете управлять кодом и заданиями автоматизации с помощью стандартных API — Livy, HS2 и т. д.
2.  **Аудит** — используя SSH, вы не можете выполнять проверять, какие пользователи получили SSH-доступ к кластеру. Этого можно избежать, создавая задания через стандартные конечные точки, так как они будут выполняться в контексте пользователя. 



### <a name="beeline"></a>Использование Beeline 
Установите Beeline на компьютер подключитесь через общедоступный сегмент Интернета, используя следующие параметры: 

```
- Connection string: -u 'jdbc:hive2://<clustername>.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'
- Cluster login name: -n admin
- Cluster login password -p 'password'
```

Если Beeline установлен локально и подключение устанавливается через виртуальную сеть Azure, используйте следующие параметры: 

```
- Connection string: -u 'jdbc:hive2://<headnode-FQDN>:10001/;transportMode=http'
```

Чтобы найти полное доменное имя головного узла, используйте сведения из руководства по получению полного доменного имени для узлов кластера.














## <a name="users-of-domain-joined-hdinsight-clusters"></a>Пользователи в присоединенном к домену кластере HDInsight.
Кластер HDInsight, который находится вне домена, имеет две учетные записи пользователя, которые создаются вместе с кластером.

* **Администратор Ambari**: эта учетная запись называется также *Пользователь Hadoop* или *Пользователь HTTP*. Эта учетная запись используется для входа на Ambari по адресу https://&lt;имя_кластера >.azurehdinsight.net. Ее также можно использовать для выполнения запросов по представлениям Ambari, выполнения заданий с помощью внешних средств (PowerShell, Templeton, Visual Studio и т. п.) и для проверки подлинности с помощью драйвера Hive ODBC и средств бизнес-аналитики (Excel, PowerBI или Tableau).

Для кластера HDInsight, присоединенного к домену, в дополнение к администратору Ambari создаются еще три пользователя.

* **Администратор Ranger**: это локальная учетная запись администратора Apache Ranger. Она не является пользователем домена Active Directory. Эту учетную запись можно использовать для настройки политик, создания других пользователей-администраторов или делегированных администраторов (чтобы они могли управлять политиками). По умолчанию используется имя пользователя *admin* и такой же пароль, как для администратора Ambari. Этот пароль можно изменить на странице настроек в Ranger.
* **Пользователь домена и администратор кластера**: это пользователь домена Active Directory, который является администратором кластера Hadoop, а также служб Ambari и Ranger. Учетные данные этого пользователя нужно предоставить во время создания кластера. Этот пользователь имеет следующие права.

  * Присоединение компьютеров к домену и помещение их в определенное подразделение, которое указывается во время создания кластера.
  * Создание субъектов-служб в определенном подразделении, которое указывается во время создания кластера.
  * Создание обратных записей DNS.

    Обратите внимание, что эти права есть и у других пользователей AD.

    В пределах кластера существуют некоторые конечные точки (например, Templeton), которые не управляются службой Ranger, и следовательно не являются безопасными. Такие конечные точки закрываются для всех пользователей, за исключением пользователя домена и администратора кластера.
* **Обычный**: во время создания кластера можно указать несколько групп Active Directory. Пользователи в эти группах будут синхронизированы с Ranger и Ambari. Эти пользователи являются пользователями домена и будут иметь доступ только к конечным точкам, которые управляются службой Ranger (например, Hiveserver2). Для этих пользователей применяются все политики RBAC и правила аудита.

## <a name="roles-of-domain-joined-hdinsight-clusters"></a>Роли в присоединенном к домену кластере HDInsight
В домене HDInsight существуют следующие роли:

* администратор кластера;
* оператор кластера;
* администратора служб;
* оператор службы;
* пользователь кластера.

**Просмотр разрешений для этих ролей**

1. Откройте пользовательский интерфейс управления Ambari.  См. раздел [Вход в пользовательский интерфейс управления Ambari](#open-the-ambari-management-ui).
2. В меню слева щелкните **Роли**.
3. Щелкните синий вопросительный знак, чтобы просмотреть разрешения.

    ![Роли в присоединенном к домену кластере HDInsight](./media/apache-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a>Вход в пользовательский интерфейс управления Ambari

1. Выполните вход на [портал Azure](https://portal.azure.com).
2. Откройте кластер HDInsight. См. раздел [Отображение кластеров](../hdinsight-administer-use-management-portal.md#list-and-show-clusters).
3. В меню сверху щелкните **Панель мониторинга**, чтобы открыть Ambari.
4. Войдите в Ambari, используя имя пользователя и пароль пользователя домена и администратора кластера.
5. Щелкните раскрывающееся меню **Администратор** в правом верхнем углу, затем нажмите кнопку **Управление Ambari**.

    ![Управление Ambari для присоединенного к домену кластера HDInsight](./media/apache-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    Пользовательский интерфейс выглядит примерно так:

    ![Интерфейс управления Ambari для присоединенного к домену кластера HDInsight](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a>Список пользователей домена, синхронизированных из Active Directory
1. Откройте пользовательский интерфейс управления Ambari.  См. раздел [Вход в пользовательский интерфейс управления Ambari](#open-the-ambari-management-ui).
2. В меню слева щелкните **Users**(Пользователи). Вы увидите всех пользователей, синхронизированных в кластер HDInsight из Active Directory.

    ![Список пользователей в интерфейсе управления Ambari для присоединенного к домену кластера HDInsight](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a>Список групп домена, синхронизированных из Active Directory
1. Откройте пользовательский интерфейс управления Ambari.  См. раздел [Вход в пользовательский интерфейс управления Ambari](#open-the-ambari-management-ui).
2. В меню слева щелкните **Группы**. Вы увидите все группы, синхронизированные в кластер HDInsight из Active Directory.

    ![Список групп в интерфейсе управления Ambari для присоединенного к домену кластера HDInsight](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a>Настройка разрешений для представлений Hive
1. Откройте пользовательский интерфейс управления Ambari.  См. раздел [Вход в пользовательский интерфейс управления Ambari](#open-the-ambari-management-ui).
2. В меню слева щелкните **Представления**.
3. Щелкните **HIVE**, чтобы открыть подробную информацию.

    ![Представления Hive в интерфейсе управления Ambari для присоединенного к домену кластера HDInsight](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. Щелкните ссылку **Представление Hive**, чтобы настроить представления Hive.
5. Выполните прокрутку вниз до раздела **Разрешения**.

    ![Настройка разрешений для представлений Hive в интерфейсе управления Ambari для присоединенного к домену кластера HDInsight](./media/apache-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. Щелкните **Добавить пользователя** или **Добавить группу**, а затем укажите пользователей или группы, которые могут использовать представления Hive.

## <a name="configure-users-for-the-roles"></a>Настройка ролей для пользователей
 Список ролей и разрешений см. в разделе [Роли в присоединенном к домену кластере HDInsight](#roles-of-domain---joined-hdinsight-clusters).

1. Откройте пользовательский интерфейс управления Ambari.  См. раздел [Вход в пользовательский интерфейс управления Ambari](#open-the-ambari-management-ui).
2. В меню слева щелкните **Роли**.
3. Щелкните **Добавить пользователя** или **Добавить группу**, чтобы назначить роли для пользователей и групп.

## <a name="next-steps"></a>Дополнительная информация
* Сведения о настройке присоединенного к домену кластера HDInsight см. в [этой статье](apache-domain-joined-configure.md).
* Сведения о настройке политик Hive и выполнении запросов Hive для присоединенного к домену кластера HDInsight см. [здесь](apache-domain-joined-run-hive.md).
