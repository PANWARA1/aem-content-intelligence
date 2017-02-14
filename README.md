# aem-content-intelligence
This plug-in helps content authors to tag content automatically based on the content. This plugin is intelligent enough to detect language entered by authors and analyze the content by using Apache Natural Language processing tool. This is a powerful feature for content authors to structure content based on the auto created tags. Content authors do not need to tag pages manually.   

This plugin uses the machine learning algorithm to detect 3 different entities:
•	Person
•	Location
•	Organization

So, once an author create content then this plugin will analyze and extract the content based on the above 3 criteria. 
After text analyzation is complete, then this plugin will create SPARQL query to get the recognized entities from DBPedia which is again based on Semantics web technology. 

After extracting the entities, this plugin will auto create tags and associate those tags to the newly created page. This also create links to Wikipedia for the recognized entities. This is another powerful feature that site visitor will get to know more information about the entities. 

Demo video is available here: https://www.youtube.com/watch?v=aohXk0ar90Q
Now let’s see how this plugin works: 
The starting point of this plugin is TextProcessor.java which implements SlingPostProcessor
TextProcessor.java detects the language from DetectLanguage.java. The default languages which are configured “English” and “Dutch”. LanguageConfigurationService OSGI service provides the supported languages that the plugin can handle. Admin has to configure this service through Admin console. 

Once the language is detected then it calls OSGI service OpenNlp.java. This OSGI service is intelligent to initialize models (person/location/organization) based on the detected language. Now the models(which are of type person/location/organization) , gets the input from another OSGI service called FileProvider.java

The way FileProvider service works is interesting. The Activator class instantiate FileBundleInstaller class and it listen to all the bundles and filter out only the required bundles which meets the criteria “Data-files”. Whenever any bundle is uploaded with headers as “Data-files” then FileBundleInstaller registers that bundle which provides the backed for data extraction. The advantage of doing is that, we do not need to configure physical file path. 

SPARQL.java file executes the query against DBPedia to get the required entities and returns inform of HashMap which then further processed by AEM using TagManager API to creates tags and update page properties.  

