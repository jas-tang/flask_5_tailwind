# flask_5_tailwind
This is a repository for Assignment 5 in HHA504. 

## Creating the App

This was a very simple flask app that is customized with tailwind CSS. The goal of this assignment was to host a video source in a CDN. 

The base app.py file is this simple:
```
from flask import Flask, render_template

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index_tailwind.html')

if __name__ == '__main__':
    app.run(debug=True)
```

The tailwind css file called "index_tailwind" has a little bit more customization.

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flask App</title>
    <!-- Include Tailwind CSS from CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
</head>

<!-- Top Bar Nav -->
<nav class="max-w-screen-md py-4 bg-blue-800 shadow mx-auto">
    <div class="max-w-screen-md container mx-auto flex flex-wrap items-center justify-between">

        <nav>
            <ul class="flex items-center justify-between font-bold text-sm text-white uppercase no-underline text-center mx-auto">
                <li><a class="hover:text-gray-200 hover:underline px-4" href="#">Home</a></li>
                <li><a class="hover:text-gray-200 hover:underline px-4" href="https://github.com/jas-tang/flask_5_tailwind">Github</a></li>
            </ul>
        </nav>
    </div>
</nav>

<body class="bg-gray-800 p-4">
    <div class="max-w-screen-md mx-auto bg-gray p-4">
        <h1 class="text-2xl font-bold mb-4 text-center text-white">Flask App Showcase</h1>

        <video controls class="mx-auto mb-4" width="640" height="360">
            <source src="https://jason-cdn.azureedge.net/jason-flask-app/gcpazureflask.mp4" type="video/mp4">
            Your browser does not support the video tag.
        </video>
           
</body>

<footer class="max-w-screen-md py-4 bg-white shadow mx-auto">
    <img src="https://www.stonybrookmedicine.edu/sites/default/files/cckimages/page/sbm_horz_1clr1.jpg"  alt="Image" class="mx-auto mb-4 items-center" style="width: 200px; height: 40px;">

</footer>

</html>
```
There is a top bar for navigation. Home leads nowhere. Github leads to my Github repository. 

The next section is the body which contains the video that connects to my CDN on Azure (more on that later). 

The last section is the footer, which just contains an image of Stony Brook Medicine's logo that I found on google image search. 

## Setting up the Cloud CDN

I set up a storage account on Microsoft Azure. This acts as a repository for providing images, videos, data, etc. It is similar to a Google Drive or DropBox.

![](https://github.com/jas-tang/flask_5_tailwind/blob/main/images/storageaccount.JPG)

I then set up a container within the storage account.

![](https://github.com/jas-tang/flask_5_tailwind/blob/main/images/storageaccount.JPG)

This is where I can upload any file types. I uploaded my video here.

![](https://github.com/jas-tang/flask_5_tailwind/blob/main/images/storagecontainer.JPG)

As you can see, there are three different video formats of the same video. I found out that Azure does not support mkv or mov video format. I had to convert my video into a m9f file.

To actually use this as a CDN, I then created a front door end point. 

![](https://github.com/jas-tang/flask_5_tailwind/blob/main/images/frontdoorandcdn.JPG)

The video source is the following link which is hosted in a CDN: https://jason-cdn.azureedge.net/jason-flask-app/gcpazureflask.mp4

![](https://github.com/jas-tang/flask_5_tailwind/blob/main/images/CDN.JPG)

## Cloud Deployment

I deployed my web app on Microsoft Azure for the continuity. 

I first installed Azure CLI: 
```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

Then I tested if the install worked correctly with the following code: 
```
az
```

I gave permission to the shell with a Microsoft account: 
```
az login --use-device-code
```

Next, I located all subscription IDs within the shell
```
az account list --output table
```

Then, change the subscription approrpiate name. For my case, it was Azure for Students.

```
az account set --subscription (insert the subscription ID here)
```

Create a new resource group within the Azure Web Portal. It is located in the Resource Group tab. Ensure that the new group has its subscription set to the working subscription name. For my case, it was Azure for Students. 

Create the web app and connect it to Microsoft Azure with the following code.
```
az webapp up --resource-group (insert group name) --name (insert your app name) --runtime <(Insert language)> --sku <(insert service plan)>
az webapp up --resource-group Jason504 --name jastang504-flask --runtime Python:3.9 --sku b1
```

The application should now be deployed. Access your application via Microsoft Azure's App Services tab. 

In the case that you need to redeploy the app, use the following code
```
az webapp up
```

This [link](https://jastang504-flask.azurewebsites.net/) leads you to my working deployed application. 
If I deleted the the service to conserve cost, the following is an image of my working application. 
![](https://github.com/jas-tang/flask_5_tailwind/blob/main/images/workingapp.JPG)
