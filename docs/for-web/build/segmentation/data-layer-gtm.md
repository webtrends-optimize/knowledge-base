# Segment by GTM Data Layer

In Webtrends Optimize, we can ingest 3rd party data layers like GTM's, and consume this into the platform for use with Segmentation and Locations. 

## Consume the GTM Data Layer

Full details of how to pull the GTM data layer can be found here:
[Ingesting Data Layers](/for-web/getting-started/tag-implementation/ingesting-datalayers/).

Make sure you've done this. 

## Targeting by GTM Data Layer values 

Once you have set this up, the names you see in GTM's Data Layer will match the ones you would use in our segment builder.

For example, `"loyalty_status":"active"` in GTM's Data Layer would translate to **Data Object Attribute** with a name of `loyalty_status` and a value of `active`. 

Simple!