# Apa itu Maven?
Apache Maven adalah sebuah alat manajemen proyek dan comprehension tool untuk proyek berbasis Java. Maven menyediakan cara standar untuk membuat dan mengelola proyek Java, termasuk:

Mengelola dependensi library yang dibutuhkan.
Membuat dan mengkompilasi proyek menggunakan konfigurasi yang dapat diatur di file pom.xml.
Mengotomatisasi pengemasan aplikasi ke dalam format seperti WAR atau JAR.
Mengapa Harus Menggunakan Maven untuk JSP dan Servlet?
Maven sangat berguna dalam pengembangan aplikasi berbasis JSP dan Servlet karena memudahkan dalam:

Mengelola dependensi servlet dan JSP API.
Memastikan aplikasi dapat dikemas ke dalam format WAR untuk di-deploy ke server Tomcat atau server Java lainnya.
Mengotomatisasi proses build dan testing.

---
## Buat Proyek Maven di VScode
Jalankan perintah :  
```mvn archetype:generate -DgroupId=com.example -DartifactId=jsp-servlet-example -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=false```

Penjelasan perintah :
- ```mvn archetype:generate```: Perintah untuk membuat projek Maven berdasarkan template (```archetype```)
- ```-DgroupId=com.example```: Group ID untuk organisasi
- ```-DartifactId=nama-proyek```: Nama projek
- ```-DarchetypeArtifactId=maven-archetype-webapp```: Template untuk aplikasi web berbasis Java
- ```DinteractiveMode=false```: Untuk menjalankan perintah tanpa dialog interaktif

---
## Modifikasi pom.xml
```
<dependencies>
    <!-- Servlet API -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
        <scope>provided</scope>
    </dependency>

    <!-- JSP API -->
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>javax.servlet.jsp-api</artifactId>
        <version>2.3.3</version>
        <scope>provided</scope>
    </dependency>
</dependencies>

<build>
    <plugins>
        <!-- Plugin untuk menjalankan proyek menggunakan Tomcat -->
        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>
            <configuration>
                <port>8080</port>
                <path>/</path>
            </configuration>
        </plugin>
    </plugins>
</build>
```

---
## Buat Servlet dan JSP File
- Buat kelas **Servlet** di dalam folder :  
```src/main/java/com/example/HelloServlet.java : ```  
```
package com.example;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/hello")
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("text/html");
        response.getWriter().println("<h1>Hello from Servlet!</h1>");
    }
}
```

- Buat file dengan nama ```index.jsp``` di dalam folder ```src/main/webapp``` :  
```
<html>
<head>
    <title>JSP Example</title>
</head>
<body>
    <h1>Welcome to JSP and Servlet Example</h1>
    <a href="hello">Go to HelloServlet</a>
</body>
</html>
```

---
##  Konfigurasi ```web.xml```
- Buat file ```web.xml``` di folder ```src/main/webapp/WEB-INF``` :  
```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

    <servlet>
        <servlet-name>HelloServlet</servlet-name>
        <servlet-class>com.example.HelloServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>HelloServlet</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>
```

## Cara kan Proyek JSP dan Servlet
### Dengan File WAR
- Build proyek dengan perintah ```mvn clean package```
- Salin file WAR ke folder ```xampp/tomcat/webapp```
- Jalan Apache Tomcat dari XAMPP Control Panel
- Akses proyek melalui browser ```http://localhost:8080/nama-kelas```

### Dengan Maven Tomcat Plugin
- Jalankan perintah ini di terminal ```mvn tomcat7:run```
- Lalu Maven akan menjalankan Tomcat di portt 8080, dan kalian bisa mengakses proyek di ```http://localhost:8080/```

#### Jika mendapati error ini
```[ERROR] No plugin found for prefix 'tomcat7' in the current project and in the plugin groups [org.apache.maven.plugins, org.codehaus.mojo] available from the repositories [local (C:\Users\chris\.m2\repository), central (https://repo.maven.apache.org/maven2)] -> [Help 1]
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/NoPluginFoundForPrefixException
```
- Tambahkan Plugin Tomcat di ```pom.xml```
```
<build>
    <plugins>
        <!-- Plugin untuk menjalankan Tomcat -->
        <plugin>
            <groupId>org.apache.tomcat.maven</groupId>
            <artifactId>tomcat7-maven-plugin</artifactId>
            <version>2.2</version>
        </plugin>
    </plugins>
</build>
```

# TES SAJA !!!