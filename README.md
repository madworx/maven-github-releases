# Enabling Maven to pull dependencies from Github Releases

If you're keeping projects that use Maven on GitHub, and wish to use GitHub's releases feature to publish your artifacts, you'll notice that the GitHub releases URL structure isn't compatible with what Maven expects.

Enter a simple web redirection service: (Apache web server configuration snippet)

```
<VirtualHost *:443>
   ServerName maven.madworx.se
   ...
   RewriteEngine on
   RewriteRule /com/github/([^/]*)/([^/]*)/([^/]*)/([^/]*) https://github.com/$1/$2/releases/download/$3/$4 [L]
   ...
</VirtualHost *:443>
```

Then add the following to your pom.xml file: (Instructing Maven to query your service if the package isn't found at Maven central)
```
   <repositories>
      <repository>
         <id>madworx</id>
         <url>https://maven.madworx.se</url>
      </repository>
   </repositories>
```

And add dependency information for the artifacts published to github releases:
```
   <dependencies>
      <dependency>
         <groupId>com.github.madworx</groupId>
         <artifactId>SwingLibrary</artifactId>
         <version>1.9.8mwx14</version>
      </dependency>
  </dependencies>
```

Martin Kjellstrand <martin.kjellstrand@madworx.se>
