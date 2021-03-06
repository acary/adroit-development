---
description: >-
  This section describes a technical overview of starting a new project with
  Java, Spring MVC, and JPA.
---

# Full Stack MVC

### Overview

* Initialize local workspace
* Create Git repository
* Create and configure JPA project
* Create and configure Spring Boot project
* Connect Spring Boot with JPA project
* _Create and configure MySQL database_
* _Update Controllers_
* _Write JUnit test and run smoke test_

#### Initialize local workspace

Start by preparing space on your local environment where your project files will be stored.

*   Open a terminal

    * Change directory to where your workspace will be
      * `cd ~/Documents/sites`
    * Create `.gitignore` file
      * `atom .gitignore`
    * Add the following and save the file:

    ```
    .DS_Store
    Servers
    target
    build
    bin
    .metadata
    # .settings
    .gradle
    *.war
    *.bak
    ```

#### Create Git repository

We will use Git as the version control system.

* Create a new repository at GitHub.com
* Initialize your local folder as a `git` repository and perform an initial commit:

```
echo "# MyProject" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/myUsername/MyProject.git
git push -u origin main
```

#### Create and configure JPA project

Set up the Java Persistence API project in Spring Tool Suite (STS).

* Set or confirm in STS > `Preferences`:
  * `Gradle > Specific Gradle version: 7.4.2`
  * `Java > Compiler > Compiler compliance level: 1.8`
  * `General > Workspace > Show full workspace path`
  * `Console (search for) > Limit console output (uncheck)`
* Create JPA Project
  * `File > New > Other > JPA Project > Next`
  * Add source folders to structure:
    * `src/main/java`
    * `src/main/resources`
    * `src/test/java`
    * `src/test/resources`
  * Click Next
    * `JPA Implementation > Disable Library Configuration (select)`
  * Finish (don't open JPA Perspective if prompted)
* In the `Project Explorer` panel:
  * Drag the `META-INF` folder from `src/main/java` down one level to `src/main/resources`

Add Gradle using the following steps:

* Right-click your project folder and select `Configure > Add Gradle Nature`
* Reveal Gradle Tasks by clicking: `Window > Show View > Other > Gradle Tasks`
* On the Gradle Tasks panel, select: `Build > init (double-click)`
* Open the Console panel: accept defaults for the 4 prompts by hitting `enter`
* Right-click the project folder and select: `Refresh`

Update `build.gradle`:

```
apply plugin: 'java'
apply plugin: 'eclipse'

/*
name your project's dependency group and give it an initial version.
the 'artifactId' is assigned automatically based on the project's name.
*/
group = 'com.myorganization'
version = '0.0.1-SNAPSHOT'

// JDK versioning for compilation and generated bytecode
sourceCompatibility = 1.8
targetCompatibility = 1.8

eclipse {
  classpath {
    downloadSources = true
  }
}

// set query location for dependencies
repositories {
  jcenter()
}

/*
 set the version of hibernate to use to keep your dependencies DRY and
 consistent across artifactIds
*/
ext {
  hibernateVersion = "5.4.32.Final"
  mySqlConnectorVersion = "8.0.27"
  junit5Version = "5.8.2"
}

// define project specific dependencies
dependencies {
  implementation "mysql:mysql-connector-java:$mySqlConnectorVersion"
  implementation "log4j:log4j:1.2.17"
  implementation "org.hibernate:hibernate-core:$hibernateVersion"
  implementation "org.hibernate:hibernate-c3p0:$hibernateVersion"
  
  testImplementation("org.junit.jupiter:junit-jupiter:$junit5Version")
}

test {
    useJUnitPlatform()
}
```

Run `Gradle Refresh` by right-clicking the project and selecting `Gradle > Refresh Gradle Project`

Create `log4j.properties` file under `src/main/resources` and `src/test/resources` with the following contents:

```
// Logging levels, from least verbose to most, are:
// OFF, FATAL, ERROR, WARN, INFO, DEBUG, TRACE, and ALL
// Change this line from WARN to INFO for more detailed logging
log4j.rootLogger=WARN, stdout
log4j.logger.org.hibernate.type=WARN
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m%n
```

Under the `META-INF` folder, update `persistence.xml` using the following as a guideline:



```
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.2"
	xmlns="http://xmlns.jcp.org/xml/ns/persistence"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/persistence http://xmlns.jcp.org/xml/ns/persistence/persistence_2_2.xsd">
	<persistence-unit name="JPAMyProject">
		<provider>org.hibernate.jpa.HibernatePersistenceProvider
    </provider>

		<!-- List Entities Here -->
		<class>com.myorganization.myproject.entities.User</class>

		<properties>
			<property name="javax.persistence.jdbc.url"
				value="jdbc:mysql://localhost:3306/MYPROJECTDB?useSSL=false&amp;useLegacyDatetimeCode=false&amp;serverTimezone=US/Mountain" />
			<property name="javax.persistence.jdbc.user" value="MYUSER" />
			<property name="javax.persistence.jdbc.password" value="MYPASSWORD" />
			<property name="javax.persistence.jdbc.driver" value="com.mysql.cj.jdbc.Driver" />

			<property name="hibernate.show_sql" value="true" />
			<property name="hibernate.format_sql" value="true" />

			<property name="hibernate.dialect"
				value="org.hibernate.dialect.MySQLDialect" />
			<property name="hibernate.connection.provider_class"
				value="org.hibernate.connection.C3P0ConnectionProvider" />

			<property name="hibernate.c3p0.max_size" value="5" />
			<property name="hibernate.c3p0.min_size" value="0" />
			<property name="hibernate.c3p0.acquire_increment" value="1" />
			<property name="hibernate.c3p0.idle_test_period" value="300" />
			<property name="hibernate.c3p0.max_statements" value="0" />
			<property name="hibernate.c3p0.timeout" value="60" />
		</properties>

	</persistence-unit>
</persistence>
```

Copy the fully-qualified name from your listed entity and navigate to `src/main/java (right-click) > New > Class`

In the `Name`, paste the text from your clipboard as copied from the `persistence.xml` file above (it should auto-populate the package name), i.e. `com.myorganization.myproject.entities.User`

**Stub out JUnit test**

* Create a new package under `src/test/java` for entities, i.e. `com.myorganization.myproject.entities`
* Create `New > JUnit Test`&#x20;
  * Ensure `JUnit Jupiter Test` is selected
  * Name: `UserTest`
  * Add: `@Before` and `@After` annotations

Run Gradle refresh again, then commit and push your updates to the project repository.

#### Create and configure Spring Boot project

* Get the package name from the JPA project you just created, i.e. `com.myorganization.myproject`
* In STS, click `New > Spring Starter Project`
  * Project name: _Name of your project_
  * `Gradle buildship 2.x`
  * Package name: _Must match with JPA project_
  * Create packages and add empty stub class to each package
  * Include Spring dependencies:
    * _Spring Web_
    * Spring Data JPA
    * MySQL Driver
  * Click `Finish`

Configure JPA and JSTL dependencies. In `build.gradle` add the following:

```
implementation 'javax.servlet:jstl:1.2'
implementation 'org.apache.tomcat.embed:tomcat-embed-jasper'
```

Save and perform Gradle refresh.

#### **Connect Spring Boot and JPA Projects**

* JPA project**:** `settings.gradle` > copy project name, i.e. `'JPAMyProject'`
* Boot project: `settings.gradle` > add: `includeFlat 'JPAMyProject'`
* Boot project: `build.gradle` > add: `implementation project(':JPAMyProject')`

Save all files and perform another Gradle refresh. Verify by expanding the `Projects and External Dependencies` folder (in the Spring Boot project) and see `JPAMyProject` at the bottom.

Commit (`Create Spring Boot MVC Project, add JPA Project dependency`) and push.

#### **Create web folder**

Create the `webapp/WEB-INF` folder under `src/main`

* In Package Explorer, expand `src/main` and right-click to select `New > Folder` and give it a name: `webapp/WEB-INF`&#x20;
* In it, create `New > Other > JSP File` and give it a name: `home`
* Add a tag to pass data into from the backend: `${DEBUG}`

#### Create Packages

Packages are folders that house separate concerns in a project. Create the following packages in the same level as `com.myorganization.myproject:`

* _Controllers_
  * `com.myorganization.myproject.controllers`
    * `HomeController.java`
* _Data_
  * `com.myorganization.myproject.data`
    * `UserDAO.java`
    * `UserDaoJpaImpl.java`

**Controllers**

```
new HomeController
@Controller
@RequestMapping(path = {"/", "home.do"})
```

**Data**

In the `data` package, select `New > Interface` and give it a name (i.e. `UserDAO`

Add a method to find a user by ID (import with `CMD + O`)

`User findById(int userId);`

In the package explorer, expand and right-click the purple `I` icon and select `New > Class` and give it a name:

`UserDaoJpaImpl`

* `@Service`
* `@Transactional`&#x20;
* `@PersistenceContext`&#x20;
  * `private Entity Manager em;`

File `UserDaoImpl.java`

```
package com.myorganization.myproject.data;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import javax.transaction.Transactional;

import org.springframework.stereotype.Service;

import com.myorganization.myproject.entities.User;

@Service
@Transactional
public class UserDaoImpl implements UserDAO {

	@PersistenceContext
	private EntityManager em;

	@Override
	public User findById(int userId) {
		return em.find(User.class, userId);
	}

}
```

#### Update application.properties

File `src/main/resources/application.properties`&#x20;

```
#### PORT CONFIG ####
##### Alternate Tomcat port
server.port=8081


#### JSP VIEW RESOLVER ####
##### Include if you used a ViewResolver to shorten the names of your views
spring.mvc.view.prefix: /WEB-INF/
spring.mvc.view.suffix: .jsp


#### MYSQL DATASOURCE ####
##### Configure to match your Database
spring.datasource.url=jdbc:mysql://localhost:3306/FIXME?useSSL=false&useLegacyDatetimeCode=false&serverTimezone=US/Mountain
spring.datasource.username=FIXME
spring.datasource.password=FIXME
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver


#### LOGGING ####
##### Set to WARN for fewer log messages
logging.level.root=WARN
logging.level.org.springframework.web=DEBUG
logging.level.org.hibernate.SQL=WARN
logging.level.org.hibernate.type=WARN
spring.jpa.show-sql=true


#### TOMCAT ####
spring.datasource.tomcat.max-active=10
spring.datasource.dbcp2.max-idle=8
spring.datasource.dbcp2.max-wait-millis=10000
spring.datasource.dbcp2.min-evictable-idle-time-millis=1000
spring.datasource.dbcp2.min-idle=8
spring.datasource.dbcp2.time-between-eviction-runs-millis=1
```

_Commit and push_

`git commit -m 'Configure Spring, stub MVC code'`

`git pull`&#x20;

`git push`
