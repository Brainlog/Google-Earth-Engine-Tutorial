Now, we look at the code of Earth Engine and step by step understand what would be happening in the background. 
To make the script in Google Earth Engine refer to Practice section.    
This code is written in Java Script on code.earthengine.google.com.

## Example Code     
```
var collection = ee.ImageCollection("LANDSAT/LC08/C02/T1_L2");
```

In this code snippet, we are using inbuilt dataset of GEE 'Landsat'. 'ee' is used for calling earth engine library. ImageCollection is a constructor which takes the path of Dataset you want to load.          

Each element in this image collection represents an image. GEE provides a wrapper over every data it stores and this abstraction is called as object or more specifically container. Every Image or an Image collection is stored as a dictionary. This dictionary would be referred as an object.  

Each object has some attributes (which could be referred as Meta Data) and actual data as keys.     

You can also use the methods provided by GEE to manipulate an object (Image or Image Collection) in several ways.   

Typically these manipulations could be expensive. For eg., Your are asked to normalize each image in this Image collection, so that each pixel values are mapped between 0 to 1.        

What could be the most basic way to solve this task?    
1. If you think this Image Collection as dictionary, we can iterate over all the keys of this Image Collection. The keys of this Image Collection would be Images. 
2. And in every iteration, you take the key, and for each pixel, divide the value of pixel by its maximum possible value.
3. Any iteration could be done by a for loop.

But this method could be very slow to perfom any analysis even at a state level. Let us understand what would happen if you use above algorithm to perform this. 

Remember that when you use the standard java script to perform the computation, all the data required to perform this computation is first collection to your device and then computed locally. So, one thing is clear, we can't just use for loop to do this. The size of this Image Collection is more than 1 TB!     

Let's now see, how we can use Earth Engine at its fullest :     
1. We redesign this algorithm to use distributed computing.
2. We think this task of performing normalization on each image as a manipulation on Image Collection. 
3. GEE provides a different programming way to perform this manipulation in a distributed fashion.
4. To perform this manipulation over Image Collection, we need to manipulate each image so that all pixels are normalized.  
5. To manipuate large Collections in a distributed fashion, GEE provides methods like map and reduce. 

### Map Method For ImageCollection
This method takes as input an Image Collection and a custom function which should act on each element of Image Collection.      
When we run this method on any Image Collection, Earth Engine Master Server will perform this computation on its own server and no dataflow would take place between your browser and master server.        
Now, we also have to write a function which should be passed as input for this Map method.      
What should this function do? It should manipulate the Image (base object of Image Collection). 
```
var manipulate_base_object = function(base_object){
    return base_object.divide(255);
}
var manipulated_collection = collection.map(manipulate_base_object)
```

The above code performs the task without using any for loop and using the Earth Engine at its full.     

Remember that any object of Earth Engine can only be manipulated by the methods of that object provided by Earth Engine.
In our case, to normalize the image, we need to use its method (.divide(255)) which divides each pixel by 255 and returns a new image.      

Now Let's click on run button. This task will be completed just in milliseconds!! Why?? This Image Collection size is more than 1 TB. Even after distributing this is not possible only with thousand remote machines.      

What's actually happening?      
When you click on run button, it does not execute this code. 

Why Earth Engine do this?       
Clicking on 'Run' button and looking at the output on console is referred as Interactive computation. Earth Engine assumes that these type of computation are only performed for debugging purposes and are not optimized for performing computation on large input.  
Hence, if you not print something related to that computation, it will not execute it.      

Let us change the above code to print the output of first image :       
```
var manipulate_base_object = function(base_object){
    return base_object.divide(255);
}
var manipulated_collection = collection.map(manipulate_base_object);
print(manipulated_collection.first());
```







