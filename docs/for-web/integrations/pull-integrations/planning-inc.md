# Planning Inc - Pull Integration

Planning Inc is a data company, and we integrate with them on a "pull" basis. 

They compile segment lists from Google Analytics, and ship them to Webtrends Optimize so that we can target users based on this data.

## How it works

You will work with Planning Inc to build your segments of Google Analytics audiences.

1. Work with Planning Inc to build your audiences.
2. They will ship data to us, into a Blob Storage container in Microsoft Azure.

    A call may be required so we can share credentials with them - please speak to your Account Manager so we can arrange this.

3. We process the data. As it comes in, we'll transform the data into user-centric information ("this user belongs to segments A, B, C"). 
4. This data is made available in your tag - note that you'll need to be using a Custom Lib file with a workers.dev URL for this to work.
    
    We will add it into your Webtrends Optimize Data Layer.

5. You can use it for Locations or Segments - using the Data Object Attribute values. 

    You can also refer to these values with JavaScript - found in `WT.optimizeModule.prototype.wtConfigObj.data`.

That's it!

Most of the work will happen behind the scenes to get their segments into our data layer, and frmo that point onwards you'll be able to use the data seamlessly.