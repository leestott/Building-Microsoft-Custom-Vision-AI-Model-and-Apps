# Getting started building a custom vision indexer for a selection of images. 

So you want to build a image classifer for a hackathon or project well there tools such as [Microsoft Custom Vision AI](http://www.customvision.ai) but to make thes eeffective you need to upload and tag images for the model to be successfully trained. Using Bing Image CLI you can download the images directly to your local machine, then add or remove any other images to fine tune your image stock.  Then you can use the Custom Vision Model CLI to create a new model, upload the images and correctly label each one.  Further more, the model is trained and then made available for running quick tests - to see if you're classifer is returning the expected results.

# Task 1. Grab some images

So one of the most time consuming tasks is to find suitable images upload the suitable images and tag the images so that they are classified.

### Bing Image CLI

For more details on usage see [Bing Image CLI](BingImageCLI/Readme.md)
You will first need to create a Bing Image API Key. To retrieve or create your Bing Image API key start here: [Bing Image API](https://azure.microsoft.com/en-gb/try/cognitive-services/?api=bing-image-search-api) 

### Bing Image Search API Results - Image Downloader

To use we simply provide the arguments for the Bing Search criteria and Bing Search API key eg:

- BingImageCLI.exe -k yourkey -s _"Search topic"_ -l ShareCommercially -p _file location_ -m 50 -fmax 4000000

Here an example of downloading a set of images related to mushroom varieties

```
BingImageCLI.exe -k yourkey -s "Coprinopsis atramentaria" -l ShareCommercially -p c:\photos\mushroom -m 50 -fmax 4000000
BingImageCLI.exe -k yourkey -s "Geastrum triplex" -l ShareCommercially -p c:\photos\mushroom -m 50 -fmax 4000000
BingImageCLI.exe -k yourkey -s "Lycoperdon perlatum" -l ShareCommercially -p c:\photos\mushroom -m 50 -fmax 4000000
BingImageCLI.exe -k yourkey -s "Psilocybe cubensis, magic" -l ShareCommercially -p c:\photos\mushroom -m 50 -fmax 4000000
BingImageCLI.exe -k yourkey -s "Agaricus bisporus" -l ShareCommercially -p c:\photos\mushroom -m 50 -fmax 4000000
```

This will download over 200 images (some may fail with 403/404s) to the c:\photos\mushroom folder each within their own subfolder of category.


For a more details on the CLI see [BingImageCLI](/BingImageCLI/)

### Recommendation

You'll want to quickly scan through all the downloaded images, deleting any rogue ones - as some images may have been incorrectly labelled or will simply not be useful for your classifier.

# Task 2. Create a Custom Vision model for our classifier

## Custom Vision Model CLI - with image uploading, tagging and training

Cross platform CLI to provision a new Microsoft Custom Vision model using images stored on your local machine.

For more details on usage see [Custom Vision CLI](CustomVisionCLI/Readme.md). You will need to first create a CustomVision API Key 
Custom Vision API Key. To retrieve your [Custom Vision API key start here](https://azure.microsoft.com/en-gb/try/cognitive-services/)

To use we simply provide the arguments for the Custom Vision API eg:

- CustomVisionCLI.exe -k _yourkey_ -p _file location_ -n _customvisionName_

Example

```
CustomVisionCLI.exe -k yourkey -p c:\photos\mushroom -n Mushroom
```

This will upload all of the remaining images above, labelling them at the same time and will then train the model.  

For a more details on the CLI see [CustomVisionCLI](/CustomVisionCLI/)

# Task 3. Testing your model

After a couple of minutes you can then test the new classifier with a previously unseen image.  You can either do this in the [Customvision.ai portal](http://www.customvision.ai), or again from the CLI tool 

The command from the tool is 
CustomVisionCLI.exe -k _yourkey_ -p _imagetestlocation_" -n _customvisionName_ -

Example

CustomVisionCLI.exe -k yourkey -p c:\dump\unseen\mushroom.jpg" -n Mushroom -


# Task 4. Building a TensorFlow Light Model .NetCore App

Cross platform CLI to run a pre-trained model exported from [CustomVision.ai](https://CustomVision.ai) in the Tensorflow format for image classification using the [TensorFlowSharp library](https://github.com/migueldeicaza/TensorFlowSharp).

To learn more about Microsoft Cognitive Custom Vision Service, please see here: https://azure.microsoft.com/en-gb/services/cognitive-services/custom-vision-service/

For a demo see [CustomVision-Tensorflow-CSharp-Master](/CustomVision-TensorFlow-CSharp-master/Readme.md)

# Task 5. Putting this all together

Create a Custom Vision classifer in 30 seconds starting from nothing.  Using both of these CLI's together, you can quickly experiment with Custom Vision for anything.  Here, in this demo, the goal is to create a Custom Vision Model that can determine images of cucumbers from courgettes.  I start from scratch, first downloading images from Bing using the Bing API of courgettes and then cucumbers.  These are stored in local folders, using the search term as the folder name (comma separated - which can be tweaked after if required).  Then, a Custom Vision model is created named "CucumberOrCourgette", and the parent folder of all the images is provided.  Within the Custom Vision model, labels are created for each search term that was used (cucumber, courgette, green, vegetable).  As each folder of images is uploaded, the appropriate tags are added to each image.  The Custom Vision model is then trained and the default iteration is set. Finally, the model is tested by providing an unseen image to classify.

![Demo](Images/Bing%20Image%20and%20Custom%20Vision%20CLI.gif)

Also you can build an Mobile app or even a .Net Core console app see the example in [CoreExample](\CoreExample) which walks you through the process including developing a mobile app + console app.