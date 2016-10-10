# fuse-standalone
Packaging and deployment of OSGI bundles on Fuse Karaf standalone

## Maven
3 plugins are used:

- features-maven-plugin <br>
  Use the 'generate-features-xml' goal tp create a features.xml file based on the pom dependencies and bundles.properties

- build-helper <br>
  Attach the features.xml file to the compiled artifact

- features-maven-plugin <br>
  Use the 'create-kar' goal tp create a kar file (karaf jar) containing the current bundle + the features.xml


### Using the resulting feature
The package can be deployed to Nexus and then used in another project as a feature dependency with: <br>

&lt;dependency&gt;  <br>
    &lt;groupId&gt;appNamespace&lt;/groupId&gt;  <br>
    &lt;artifactId&gt;appName&lt;/artifactId&gt; <br>
    &lt;version&gt;x.y.z&lt;version&gt; <br>
    &lt;classifier&gt;features&lt;/classifier&gt; <br>
    &lt;type&gt;xml&lt;/type&gt; <br>
&lt;/dependency&gt; <br>

The deployed features.xml repository can be added in Karaf with: <br>
features:add-url mvn:appNamespace/appName/x.y.z/xml/features

Then any of the feature or bundle listed in the feature repository can be deployed separately: <br>
features:install ...
  
### Note
The 'generate-features-xml' goal creates a features.xml based on the maven dependencies only and doesn't include the current bundle itself. <br>
The idea is to maintain one single maven project to deploy any artifacts using features. <br>
For that purpose, a dependency defined with the maven properties "package", "application" and "version" has been introduced in the pom. <br>
To create a features.xml for a specific bundle created as a separate maven project and deployed to Nexus in isolation, <br>
simply override the package, application and version propoerties on the command-line: <br>
mven -Dpackage=... -Dapplication=... -Dversion=... deploy
