# Подготовка к выполнению
- Создайте два VM в Yandex Cloud с параметрами: 2CPU 4RAM Centos7 (остальное по минимальным требованиям).
![YC](https://github.com/EVolgina/09-ci-03-cicd/blob/main/yc.png)
- Пропишите в inventory playbook созданные хосты.
- Добавьте в files файл со своим публичным ключом (id_rsa.pub). Если ключ называется иначе — найдите таску в плейбуке, которая использует id_rsa.pub имя, и исправьте на своё.
- Запустите playbook, ожидайте успешного завершения.

- Проверьте готовность SonarQube через браузер.
- Зайдите под admin\admin, поменяйте пароль на свой.
![aq](https://github.com/EVolgina/09-ci-03-cicd/blob/main/sq.png)
- Проверьте готовность Nexus через бразуер.
- Подключитесь под admin\admin123, поменяйте пароль, сохраните анонимный доступ.
## Знакомоство с SonarQube
# Основная часть
- Создайте новый проект, название произвольное.
- Скачайте пакет sonar-scanner, который вам предлагает скачать SonarQube.
-Сделайте так, чтобы binary был доступен через вызов в shell (или поменяйте переменную PATH, или любой другой, удобный вам способ).
- Проверьте sonar-scanner --version.
```
wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
unzip -q -d ~ 
sonar-scanner-cli-5.0.1.3006-linux.zip
devops@WORKBOOK:~/ansible/mnt-homeworks/09-ci-03-cicd/example$ export PATH=$PATH:$(pwd)/sonar-scanner-5.0.1.3006-linux/bin
devops@WORKBOOK:~/ansible/mnt-homeworks/09-ci-03-cicd/example$ sonar-scanner --version
INFO: Scanner configuration file: /home/devops/ansible/mnt-homeworks/09-ci-03-cicd/example/sonar-scanner-5.0.1.3006-linux/conf/sonar-scanner.properties
INFO: Project root configuration file: NONE
INFO: SonarScanner 5.0.1.3006
INFO: Java 17.0.7 Eclipse Adoptium (64-bit)
INFO: Linux 5.15.90.1-microsoft-standard-WSL2 amd64

```
- Запустите анализатор против кода из директории example с дополнительным ключом -Dsonar.coverage.exclusions=fail.py.
- Посмотрите результат в интерфейсе.
- Исправьте ошибки, которые он выявил, включая warnings.
![bug](https://github.com/EVolgina/09-ci-03-cicd/blob/main/bag.png)
- Запустите анализатор повторно — проверьте, что QG пройдены успешно.
- Сделайте скриншот успешного прохождения анализа, приложите к решению ДЗ.
 ![nobug](https://github.com/EVolgina/09-ci-03-cicd/blob/main/nobag.png)
## Знакомство с Nexus
# Основная часть
- В репозиторий maven-public загрузите артефакт с GAV-параметрами:
- groupId: netology;
- artifactId: java;
- version: 8_282;
- classifier: distrib;
- type: tar.gz.
- В него же загрузите такой же артефакт, но с version: 8_102.
- Проверьте, что все файлы загрузились успешно.
![nexus](https://github.com/EVolgina/09-ci-03-cicd/blob/main/nexus.PNG)

- В ответе пришлите файл maven-metadata.xml для этого артефекта. [maven-metadata.xml] (https://github.com/EVolgina/09-ci-03-cicd/blob/main/meta)
## Знакомство с Maven
# Подготовка к выполнению
- Скачайте дистрибутив с maven.
- Разархивируйте, сделайте так, чтобы binary был доступен через вызов в shell (или поменяйте переменную PATH, или любой другой, удобный вам способ).
- Удалите из apache-maven-<version>/conf/settings.xml упоминание о правиле, отвергающем HTTP- соединение — раздел mirrors —> id: my-repository-http-unblocker.
- Проверьте mvn --version.
```
Wget https://dlcdn.apache.org/maven/maven-3/3.8.8/binaries/apache-maven-3.8.8-bin.zip
unzip -q -d ~apache-maven-3.8.8-bin.zip

java-1.11.0-openjdk-amd64  
java-11-openjdk-amd64
devops@WORKBOOK:~/ansible/mnt-homeworks/09-ci-03-cicd/example$ mvn --version
Apache Maven 3.8.8 (4c87b05d9aedce574290d1acc98575ed5eb6cd39)
Maven home: /home/devops/ansible/mnt-homeworks/09-ci-03-cicd/example/apache-maven-3.8.8
Java version: 11.0.20.1, vendor: Ubuntu, runtime: /usr/lib/jvm/java-11-openjdk-amd64
Default locale: en, platform encoding: UTF-8
OS name: "linux", version: "5.15.90.1-microsoft-standard-wsl2", arch: "amd64", family: "unix"
```
- Заберите директорию mvn с pom.
## Основная часть
- Поменяйте в pom.xml блок с зависимостями под ваш артефакт из первого пункта задания для Nexus (java с версией 8_282).
- Запустите команду mvn package в директории с pom.xml, ожидайте успешного окончания.
- Проверьте директорию ~/.m2/repository/, найдите ваш артефакт.
```
devops@WORKBOOK:~/.m2/repository/netology/java$ ls
8_102  8_282
devops@WORKBOOK:~/.m2/repository/netology/java/8_282$ ls
java-8_282.pom.lastUpdated
devops@WORKBOOK:~/.m2/repository/netology/java/8_102$ ls
java-8_102.pom.lastUpdated
```
- В ответе пришлите исправленный файл pom.xml. [pom.xml](https://github.com/EVolgina/09-ci-03-cicd/blob/main/pom.xml)
