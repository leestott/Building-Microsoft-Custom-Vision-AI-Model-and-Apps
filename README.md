# Getting started building a custom vision indexer for a selection of images. 

So you want to build a image classifer for a hackathon or project well there tools such as [Microsoft Custom Vision AI](http://www.customvision.ai) but to make these effective you need to upload and tag images for the model to be successfully trained. Using Bing Image CLI you can download the images directly to your local machine, then add or remove any other images to fine tune your image stock.  Then you can use the Custom Vision Model CLI to create a new model, upload the images and correctly label each one.  Further more, the model is trained and then made available for running quick tests - to see if you're classifer is returning the expected results.

You can read more and see a variety of demos at [docs.microsoft](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/?WT.mc_id=academic-0000-leestott)

- Build Demo See folder on the desktop TrainCustomImage here is a zip folder of the contents [ZipFolder of Build Demo Contents](/TrainCustomImage.zip) a pre-requirement is [DotNetCore v2](https://dotnet.microsoft.com/download/thank-you/dotnet-sdk-2.1.4-windows-x64-installer?WT.mc_id=academic-0000-leestott) to be installed on the computer to run the CLI applications.

# Task 1. Grab some images

So one of the most time consuming tasks is to find suitable images upload the suitable images and tag the images so that they are classified.

### Bing Image CLI

For more details on usage see [Bing Image CLI](BingImageCLI/Readme.md)
You will first need to create a Bing Image API Key. To retrieve or create your Bing Image API key start here: [Bing Image API](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api&WT.mc_id=academic-0000-leestott) 

![BingAPI](/Images/BingAPI.JPG)

Simply click on the Get API Key this will then will prompt you to sign into Azure Please ensure you have an [Azure for Student Subscription](http://aka.ms/azure4student)

Select sign into existing Azure Account

![BingAPIGET](/Images/BingAPIGet.JPG)

This will now load the Azure Portal where you can create your new key.

![BingIm](/Images/BingIm.jpg)

Select the following

- Name = BingImageSearch
- Pricing = F0
- Create a new resource group or select existing
- Tick the I confirm check box
- Click the create button

This will now create the necessary resources within your Azure subscription

Once the resource are created you will be presented with this screen

![BingResource](/Images/BingResource.JPG)

Now we simply need the key for the Bing Image Search

Select go to resource

![BingKey](/Images/BingKey.JPG)

Simply now select keys you will need the Key 1 for the next step.

### Bing Image Search API Results - Image Downloader

To use we simply provide the arguments for the Bing Search criteria and Bing Search API key eg:

```
BingImageCLI.exe -k yourkey -s _"Search topic"_ -l ShareCommercially -p _file location_ -m 50 -fmax 4000000
```

Here an example of downloading a set of images related to mushroom varieties for any CustomVision project you will need at least two sets of images to the evaluation each set should be downloaded to different images folder file location. 

Example1

Download two set of images to compare Flys vs Wasps

```
BingImageCLI.exe -k yourkey -s _"Fly"_ -l ShareCommercially -p c:\flyvswasp\images -m 50 -fmax 4000000
```

This creates a folder in the location c:\flyvswasp\images\fly and has 50 images of a max of 4mb per image of freely available images of fly's

```
BingImageCLI.exe -k yourkey -s _"wasp"_ -l ShareCommercially -p c:\flyvswasp\images -m 50 -fmax 4000000
```

This creates a folder in the location c:\flyvswasp\images\wasp and has 50 images of a max of 4mb per image of freely available images of wasp's

Example 2

This will download over 200 images (some may fail with 403/404s)to the c:\photos\mushroom folder each within their own subfolder of category.

```
BingImageCLI.exe -k yourkey -s "Coprinopsis atramentaria" -l ShareCommercially -p c:\photos\mushroom -m 50 -fmax 4000000
BingImageCLI.exe -k yourkey -s "Geastrum triplex" -l ShareCommercially -p c:\photos\mushroom -m 50 -fmax 4000000
BingImageCLI.exe -k yourkey -s "Lycoperdon perlatum" -l ShareCommercially -p c:\photos\mushroom -m 50 -fmax 4000000
BingImageCLI.exe -k yourkey -s "Psilocybe cubensis, magic" -l ShareCommercially -p c:\photos\mushroom -m 50 -fmax 4000000
BingImageCLI.exe -k yourkey -s "Agaricus bisporus" -l ShareCommercially -p c:\photos\mushroom -m 50 -fmax 4000000
```

For a more details on the CLI see [BingImageCLI](/BingImageCLI/)

### Recommendation

You'll want to quickly scan through all the downloaded images, deleting any rogue ones - as some images may have been incorrectly labelled or will simply not be useful for your classifier.

# Task 2. Create a Custom Vision model for our classifier

## Custom Vision Model CLI - with image uploading, tagging and training

Cross platform CLI to provision a new Microsoft Custom Vision model using images stored on your local machine.

For more details on usage see [Custom Vision CLI](CustomVisionCLI/Readme.md). You will need to first create a CustomVision API Key 
Custom Vision API Key. To retrieve your [Custom Vision API key start here](https://azure.microsoft.com/try/cognitive-services/?WT.mc_id=academic-0000-leestott)

To use we simply provide the arguments for the Custom Vision API eg:

```
CustomVisionCLI.exe -k _yourkey_ -p _file location_ -n _customvisionName_
```

Example

```
CustomVisionCLI.exe -k yourkey -p c:\photos\mushroom -n Mushroom
```

If you had multiple folder of images for the example you simply use "" to group the folders import

```
CustomVisionCLI.exe -k yourkey -p "c:\photos\mushroom" -n Mushroom
```

This will upload all of the remaining images above, labelling them at the same time and will then train the model.  

Example

```
CustomVisionCLI.exe -k bb********************** -p "c:\pics" -n hotdogs
Creating Custom Vision Project: hotdogs
Scanning subfolders within: c:\pics
Creating Tag: Hotdog
Creating Tag: legs
Uploading: c:\pics\Hotdog images...
Uploaded 50/50 images successfully from c:\pics\Hotdog
Uploading: c:\pics\legs images...
Uploaded 44/44 images successfully from c:\pics\legs
Training model
Model status: Training
Model status: Training
Model status: Training
Model status: Training
Model status: Training
Model status: Training
Model status: Training
Model status: Training
Model status: Training
Model status: Training
Model status: Training
Model status: Training
Model status: Completed
Iteration: be14fd6c-0447-467e-b9e9-21a260a01d49 set as default
Done.
Total time: 00:01:32.9939707
```

For a more details on the CLI see [CustomVisionCLI](/CustomVisionCLI/)

# Task 3. Testing your model

After a couple of minutes you can then test the new classifier with a previously unseen image.  You can either do this in the [Customvision.ai portal](http://www.customvision.ai), or again from the CLI tool 

The command from the tool is
CustomVisionCLI.exe -k _yourkey_ -p _imagetestlocation_" -n _customvisionName_ -

Example

```
CustomVisionCLI.exe -k yourkey -p "c:\dump\unseen\mushroom.jpg" -n Mushroom -q
```
Example output

```
CustomVisionCLI.exe -k bb**************** -p "c:\pics\legs\leg1.jpg" -n hotdogs -q
Custom Vision Quick test: hotdogs with image c:\pics\legs\leg1.jpg
Tag: legs Probability: 0.999996245
Tag: Hotdog Probability: 4.059012E-08
Done.
Total time: 00:00:06.7100040
```

# Task 4. Building a simple .NETCore Application 

Your model may be evolving constantly, etc. There is a prediction API available and exposed from the service as well meaning that for each model you build, you can also get an API endpoint to which you can send either an image URL or the image itself, and get back a prediction.

Setting up .NETCode make sure you're environment is setup by following the instructions here. Next, launch a terminal and create a new Console app and run

```
dotnet new console --name MyAppName
```

Then, open the [Program.CS](/CoreExample/Program.cs) in your favourite editor I have provided a sample in this repo. There are two placeholders ****** for your prediction URL and prediction key.

```
            // Request headers - replace the ******** with your valid subscription key.
            client.DefaultRequestHeaders.Add("Prediction-Key", "**************");

            // Prediction URL - replace this example URL with your valid prediction URL. URL should be in the following format
            string url = "https://southcentralus.api.cognitive.microsoft.com/*******************";
```

First you must publish your predication endpoint.
To this simply login to [CustomVisionAI](http://www.customvision.ai) 

- Select your project 
- Select Performance
- Click the Publish button

![Publish](/Images/Publish.jpg)

Next step is we need the prediction url and key for this model. 

You get these when you open the model in CustomVision.ai a

- Select Performance
- Select the little World icon labelled predication url.

You will need the predication Key endpoint URL and Predicition Key and simply replace these in the program.cs file.

![ExportModel](/CoreExample/Images/Export.PNG)

Ensure you now save the Program.CS to replace the Program.CS file created when you created the dotnet application.

To run [Program.CS](/CoreExample/Program.cs) simply ensure the file is saved in the folder open a command window Win+R type in command  in the program.cs root folder and run the following command.

```
dotnet run
```

This will load the app and ask you for a image file path
![Dotnet Core](/CoreExample/Images/dotnetrun.PNG)

#Task 5. Testing your .NET Core application output using Jsonlint.com

To view the formatted JSON simply copy and paste the output into https://jsonlint.com/

```
{"id":"864df9c1-ee88-4c32-a4f5-f8fa1c518f7a","project":"b4ea1c04-dd50-41d0-a4c1-6df74a3a54a9","iteration":"c7a4e1de-752c-4e08-8f7e-af697c14cc22","created":"2019-04-18T13:46:29.829Z","predictions":[{"probability":0.5133471,"tagId":"5321c2f6-efa8-415e-b572-3c50eaa2543b","tagName":"7"},{"probability":0.1453804,"tagId":"631870ff-7d52-4adc-81db-736551af4f2e","tagName":"3"},{"probability":0.11937803,"tagId":"fbe4c390-7457-428f-938f-27f91a1e909d","tagName":"9"},{"probability":0.08579584,"tagId":"59f96a1a-76b0-4022-b6af-d177954dde51","tagName":"4"},{"probability":0.07935606,"tagId":"f1bf9013-7029-41fc-83cb-6cb1d18c38b0","tagName":"2"},{"probability":0.06724764,"tagId":"f4498068-490d-4bc5-87de-73ca91a2a27e","tagName":"6"},{"probability":0.0451031923,"tagId":"42e76dc6-ae4c-4c2f-bad5-34b71f0a1dec","tagName":"8"},{"probability":0.0009882526,"tagId":"d34791ed-f99d-4bac-bf12-00e213aa3aab","tagName":"5"},{"probability":0.0006828679,"tagId":"ccb2497b-00aa-4bbb-bc5a-1aad311018c0","tagName":"1"},{"probability":0.0005408682,"tagId":"501e2cca-5bb2-4e08-b6c8-5d78e1c52f87","tagName":"0"}]}
{
```

This will render the following results which are better to read 

```
	"id": "864df9c1-ee88-4c32-a4f5-f8fa1c518f7a",
	"project": "b4ea1c04-dd50-41d0-a4c1-6df74a3a54a9",
	"iteration": "c7a4e1de-752c-4e08-8f7e-af697c14cc22",
	"created": "2019-04-18T13:46:29.829Z",
	"predictions": [{
		"probability": 0.5133471,
		"tagId": "5321c2f6-efa8-415e-b572-3c50eaa2543b",
		"tagName": "7"
```

As you can see in this example the probability is 0.513 so this is 51% accurate

# Additional Tasks - Building a TensorFlow Light Model .NetCore App

With the mobile devices you can now build a cross platform mobile app to to run a pre-trained model OFFLINE - You can export either an ONXX Model or TensorFlow Lite Model from [CustomVision.ai](https://CustomVision.ai) 

You can try to export a Tensorflow model for image classification using the [TensorFlowSharp library](https://github.com/migueldeicaza/TensorFlowSharp).

To learn more about Microsoft Cognitive Custom Vision Service, please see here: https://azure.microsoft.com/services/cognitive-services/custom-vision-service/?WT.mc_id=academic-0000-leestott

For a demo see [CustomVision-Tensorflow-CSharp-Master](/CustomVision-TensorFlow-CSharp-master/Readme.md)

# Putting this all together

Create a Custom Vision classifer in 30 seconds starting from nothing.  Using both of these CLI's together, you can quickly experiment with Custom Vision for anything.  Here, in this demo, the goal is to create a Custom Vision Model that can determine images of cucumbers from courgettes.  I start from scratch, first downloading images from Bing using the Bing API of courgettes and then cucumbers.  These are stored in local folders, using the search term as the folder name (comma separated - which can be tweaked after if required).  Then, a Custom Vision model is created named "CucumberOrCourgette", and the parent folder of all the images is provided.  Within the Custom Vision model, labels are created for each search term that was used (cucumber, courgette, green, vegetable).  As each folder of images is uploaded, the appropriate tags are added to each image.  The Custom Vision model is then trained and the default iteration is set. Finally, the model is tested by providing an unseen image to classify.

![Demo](Images/Bing%20Image%20and%20Custom%20Vision%20CLI.gif)

Also you can build an Mobile app or even a .Net Core console app see the example in [CoreExample](https://github.com/leestott/Building-Microsoft-Custom-Vision-AI-Model-and-Apps/tree/master/CoreExample) which walks you through the process including developing a mobile app + console app.