# Dynatrace PTC Windchill Starter Set
This repo contains files to create PTC Windchill assets in a Dynatrace tenant including application, metrics, dashboards and synthetic monitors. Check out our Dynatrace blog on PTC Windchill/ThingWorx [here](https://www.dynatrace.com/news/blog/ai-driven-observability-for-ptc-windchill-thingworx/).

In order to create these entities in Dynatrace, you will need two items:

Name | Description
------------ | -------------
Dynatrace tenant url | `Managed` https://{your-domain}/e/{your-environment-id}  <br/>`SaaS` https://{your-environment-id}.live.dynatrace.com
API Token | Required permissions outlined in the screenshot below

The API Token for the Monaco steps needs to have these minimum permissions:

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

Before deployment
* replace the URL to test (3x) in synthetic/synthetic-monitor/config.yaml

Step | Task | Notes  
---- | ---- | ----  
1 | Execute steps 1-4 from above if you hadn't already done this  
2 | Lookup the Location ID from (private) synthetic location where you wish to run the scripts from. Export this location ID as env variable named "tenant_private_location"  
2 | Check your settings with a Monaco dry-run : monaco deploy -d manifest.yaml -e tenant -p synthetic  
3 | Deploy the windchill project : monaco deploy manifest.yaml -e tenant -p synthetic  


### Build the Extension

The extension is a custom JMX extension, and must be packaged, signed and distributed.

You can use the [Add-on for VS Code](https://developer.dynatrace.com/develop/dynatrace-extensions-vscode/getting-started),  
or you can follow the steps in [Sign extensions](https://docs.dynatrace.com/docs/ingest-from/extensions20/sign-extension)

Note: the procedures above require a separate API token with different scopes than for Monaco

### Contact

Please reach out to PTC@dynatrace.com for assistance with deploying the starter set or for any questions/concerns
