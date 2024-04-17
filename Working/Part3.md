## ee.Image

This object can be considered as an dictionary which holds the data of pixel wise values with some other meta data as attributed.       

Analogous to images, ee.Image can also store multiple 2-D arrays. Each 2-D array is called a band. There could be several bands in just a single image. Each ee.Image object provides several methods to execute some popular image operations very efficiently (possibly in distributed manner in cases of Large Images). Each Attribute (meta data) of this Image is itself an object. It could be an Image itself, Image Collection, Feature, Feature Collection, ee.Number or ee.String. Image can be considered an derived object of Feature class.

### Side Note :
Whenever you print an object, it prints the meta data of that object along with meta data of the base object. 

### Feature : 
Feature object only contains attributes. These attributes can also be any object. In similar fashion,  It could be an Image itself, Image Collection, Feature, Feature Collection, ee.Number or ee.String.

### Heirarchy of Objects : 

Let us look at the typical structure of an Image Collection.            

Image Collection        
|               
|__ 'Size' (ee.Number contains 5000)        
|       
|__ 'Image0', 'Image1' ....     

Image0      
|           
|__ 'Height' (ee.Number)       
|__ 'Width'  (ee.Number)        
|__ Geometry (Feature)              
|__ 'First 2D array representing temperature' (data)    
|__ 'Second 2D array representing Vegetation Index' (data)


### ee.Geometry
GEE provides 



