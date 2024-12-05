# Dynatrace PTC Windchill Starter Set
This repo contains files to create PTC Windchill assets in a Dynatrace tenant including application, metrics, dashboards and synthetic monitors. Check out our Dynatrace blog on PTC Windchill/ThingWorx [here](https://www.dynatrace.com/news/blog/ai-driven-observability-for-ptc-windchill-thingworx/).

In order to create these entities in Dynatrace, you will need two items:

Name | Description
------------ | -------------
Dynatrace tenant url | `Managed` https://{your-domain}/e/{your-environment-id}  <br/>`SaaS` https://{your-environment-id}.live.dynatrace.com
API Token | Required permissions outlined in the screenshot below

The API Token needs to have these minimum permissions:

![GitHub Logo](/images/TokenPermissions.png)

This version of the starter set has been updated to take advantage of Monaco to deploy the configurations and the JMX extensions were regrouped into a single Extension Framework 2.0 JMX custom extension. These configurations serve as an example - make sure to adapt or extend the settings, names, patterns to your specific environment requirements.

The Monaco projects can be found in the **MonacoV2** folder.
The Extension sources can be found in the **EF20** folder.

### Monaco V2 Configuration

Step | Task | Notes  
---- | ---- | ----  
1 | Download and install [Dynatrace Monaco](https://docs.dynatrace.com/docs/deliver/configuration-as-code/monaco/installation) | Windows / MacOS / Linux  
2 | Change directory into the MonacoV2 folder  
3 | Edit the manifest.yaml file and enter your tenant's URL   
4 | Export your API token as an environment variable named "tenant_token"  
5 | Check your settings with a Monaco dry-run : monaco deploy -d manifest.yaml -e tenant -p windchill  
6 | Deploy the windchill project : monaco deploy manifest.yaml -e tenant -p windchill  

After deployment you'll have to 
* adapt the URL detection to your application's URL (or change it before deploy in windchill/builtinrum.web.app-detection).
* update the dashboard m201-hostwindchillmonitoring - adapt geolocation, and update service tiles to filter the detected services in your environment.

### Monaco V2 Synthetic script

Step | Task | Notes  
---- | ---- | ----  
1 | Execute steps 1-4 from above if you dan't already done this  
2 | Lookup the Location ID from private location where you wish to run the scripts from. export this location ID as env variable named "tenant_private_location"  
2 | Check your settings with a Monaco dry-run : monaco deploy -d manifest.yaml -e tenant -p synthetic  
3 | Deploy the windchill project : monaco deploy manifest.yaml -e tenant -p synthetic  


### 2. Manually upload the components
* Please use any tool of your convenience (Postman, curl etc.) to make POST REST calls to your Dynatrace tenant or utilize the API explorer in the Dynatrace UI. Apply all REST calls of the files in a given subfolder.
  
Method | Endpoint | Subfolder Name | Notes
------------| ----------------------------------- | --------------- | -----------------------
POST | /api/config/v1/managementZones | 01-ManagementZones |
POST | /api/config/v1/applications/web | 02-Application |
POST | /api/config/v1/applicationDetectionRules | 03-DetectionRules | Replace the application id in the json with the application id 
POST | /api/config/v1/autoTags | 04-AutoTags | 
POST | /api/config/v1/service/requestAttributes | 05-RequestAttributes |  
POST | /api/config/v1/calculatedMetrics/service | 06-MetricsService |  
POST | /api/config/v1/service/customServices/java?position=APPEND | 07-CustomServices |
POST | /api/config/v1/extensions | 08-Extensions | Extensions require uploading the zip files 
POST | /api/config/v1/dashboards | 11-Dashboards

### Contact
Please reach out to PTC@dynatrace.com for assistance with deploying the starter set or for any questions/concerns
