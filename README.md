# Cloudinary Tech Demo
![Cloudinary_Logo](/public/widget-imgs/cloudinary-logo.png)

## Introduction
### Why Cloud Storage?
Many companies have been switching to this form of data storage due to its scalability, flexibility, cost-effectiveness, and enhanced security features compared to their traditional physical storage counterparts. Since they often offer their services on a pay-per-use basis, developers have a range of options depending on which cloud service serves their needs and budget.

### Why Cloudinary?
For Project 1, we felt that managing image storage and retrieval within our own database was quite tedious. As a result, we wanted to find a solution that would minimize our manual workload. 

Cloudinary offers a [free "Programmable Media" plan](https://cloudinary.com/pricing) that has many useful features. With this plan, users are alloted 25 credits each month for their storage. The credit breakdown is further elaborated on [here](https://cloudinary.com/documentation/developer_onboarding_faq_credits). Specifically for images, this essentially provides 25GB of image storage each month. It's important to note that the free plan does have [additional limitations](https://cloudinary.com/pricing/compare-plans), specifically on the file size and number of requests, but we found that these limitations are reasonable for a project of our scale.

The features of Cloudinary that stand out to us are that they provide developers with a widget that allows users to upload images from a variety of platforms, processes the file, and returns a link to the image for the developers to store. This image can now be referenced through a URL.

## Account Setup
### Create an Account
First, you'll need to [sign up for a free Cloudinary account](https://cloudinary.com/users/register_free)
![Signup](/public/account-setup-imgs/signup.png)

### Access Your Dashboard
After signing into your account, if it's not already selected, click on the "Programmable Media" icon located in the navbar on the left

Once you're in the "Programmable Media" tab, select your "Dashboard"
![Signup](/public/account-setup-imgs/dashboard.png)

### Getting Your "Product Environment Credentials"
The content for your dashboard will appear on the right. The data we'll need is found under the "Product Environment Credentials" !block-- specifically the "API key", "API secret", and "API environment variable" to connect to our Cloudinary database.
![Signup](/public/account-setup-imgs/env.png)

| Keys  | Values |
| ------------- | ------------- |
| Cloud name  | This is the name of our product environment. It’ll be used in order to create the publicly accessible URL for uploaded media.|
| API key  | This is used with API secret in order to communicate with the Cloudinary API. |
| API secret | This is used with the API key in order to communicate with Cloudinary API. | |

**Remember to keep the API secret safe as other users could potentially alter or access your database otherwise!**


# Ensure installation of cloudinary: 
 **To install run this command in root directory: npm i @cloudinary/url-gen @cloudinary/react** 

## Cloudinary Upload Widget
For simplicity and best code-reuse, we will make a CloudinaryUploadWidget component called CloudinaryUploadWidget.jsx. We will be able to use this widget throughout our application whenever we would like users to be able to upload an image.

First, we import all necessary components from the React library. These will be used to manage state and effects in our CloudinaryUploadWidget component.<br>
```
import { createContext, useEffect, useState } from "react";
```

Then, we will create a context in order to manage our script loading state. This way we can pass data through the component without having to pass our props down manually.<br>
```
const CloudinaryScriptContext = createContext();
```

Now, we will start our CloudinaryUploadWidget function with two parameters, uwConfig and setPublicId. We will also use  state variables for tracking whether the Cloudinary script has been uploaded.<br>
```
function CloudinaryUploadWidget({ uwConfig, setPublicId }) {
    const [loaded, setLoaded] = useState(false);
```

Next, we will use useEffect in order to properly load the Cloudinary script based on the “loaded” state. Within this effect, we will look at the loaded state variable to see if the scripts have been loaded, if not, a script element is created and added to the document.<br>
```
function CloudinaryUploadWidget({ uwConfig, setPublicId }) {
    const [loaded, setLoaded] = useState(false);

    useEffect(() => {
        // Check if the script is already loaded
        if (!loaded) {
            const uwScript = document.getElementById("uw");
```

Next, we will create an initializeCloudinaryWidget function to initialize the CloudinaryUploadWidget when the script has been successfully loaded.<br>
```
function CloudinaryUploadWidget({ uwConfig, setPublicId }) {
  const [loaded, setLoaded] = useState(false);

  useEffect(() => {
    // Check if the script is already loaded
    if (!loaded) {
      const uwScript = document.getElementById("uw");
      if (!uwScript) {
        // If not loaded, create and load the script
        const script = document.createElement("script");
        script.setAttribute("async", "");
        script.setAttribute("id", "uw");
        script.src = "https://upload-widget.cloudinary.com/global/all.js";
        script.addEventListener("load", () => setLoaded(true));
        document.body.appendChild(script);
      } else {
        // If already loaded, update the state
        setLoaded(true);
      }
    }
  }, [loaded]);
```

Once the script has been loaded, we use the command window.cloudinary.createUploadWidget, with our preset uwConfig. Whenever an image is successfully uploaded, publicId will be set and we will have access to the image information.<br>
```
  const initializeCloudinaryWidget = () => {
    if (loaded) {
      var myWidget = window.cloudinary.createUploadWidget(
        uwConfig,
        (error, result) => {
          if (!error && result && result.event === "success") {
            console.log("Done! Here is the image info: ", result.info);
            setPublicId(result.info.public_id);
          }
        }
      );
```

Then, we will add an event listener to the widget to ensure the Cloudinary upload widget is properly opened on button click for asset uploads.<br>
```
  const initializeCloudinaryWidget = () => {
    if (loaded) {
      var myWidget = window.cloudinary.createUploadWidget(
        uwConfig,
        (error, result) => {
          if (!error && result && result.event === "success") {
            console.log("Done! Here is the image info: ", result.info);
            setPublicId(result.info.public_id);
          }
        }
      );

      document.getElementById("upload_widget").addEventListener(
        "click",
        function () {
          myWidget.open();
        },
        false
      );
    }
  };
```

Lastly, we return the button upload_widget, which will initialize the Cloudinary Widget once pressed, and handle publicID once the assets are properly uploaded.<br>
```
  return (
    <CloudinaryScriptContext.Provider value={{ loaded }}>
      <button
        id="upload_widget"
        className="cloudinary-button"
        onClick={initializeCloudinaryWidget}
      >
        Upload
      </button>
    </CloudinaryScriptContext.Provider>
  );
}
export { CloudinaryScriptContext };
export default CloudinaryUploadWidget;
```

At this point, we now have a React component, CloudinaryUploadWidget to create and manage uploaded assets to a user's Cloudinary account.

## Using the Upload Widget (App.js)
Now that we have the CloudinaryUploadWidget, we will show how to integrate this widget into a basic app, and display the image on our site.<br>

To start off, we want to import the CloudinaryUploadWidget and other components from the Cloudinary library to interact with Cloudinary.<br>

AdvancedImage, responsive, and placeholder are imported from the @cloudinary/react package and are used to create actual Cloudinary URLs and render images.<br>
```
import { useState } from "react";
import CloudinaryUploadWidget from "./CloudinaryUploadWidget";
import { Cloudinary } from "@cloudinary/url-gen";
import { AdvancedImage, responsive, placeholder } from "@cloudinary/react";
```

Now, we will start off our function App, and set our cloudName, and uploadPresets. We will use our data from the “Account Setup” section of this tutorial and fill it in here accordingly.<br>

“publicID” will be the ID of the uploaded asset, “cloudName” will be your own Cloudinary cloud name, and “uploadPreset” will include the upload preset for uploading images (see earlier section).<br>
```
export default function App() {
  const [publicId, setPublicId] = useState("");
  // Replace with your own cloud name
  const [cloudName] = useState("cloud_name_here");
  // Replace with your own upload preset
  const [uploadPreset] = useState("prest_name_here");
```

Next, we will initialize our uwConfig (upload widget config) state variable with our cloudName, and uploadPreset. This is a basic example, and other examples for widget configurations can be found [here](https://cloudinary.com/documentation/upload_widget_reference).<br>
```
  const [uwConfig] = useState({
    cloudName,
    uploadPreset
    // cropping: true, //add a cropping step
    // showAdvancedOptions: true,  //add advanced options (public_id and tag)
    // sources: [ "local", "url"], // restrict the upload sources to URL and local files
    // multiple: false,  //restrict upload to a single file
    // folder: "user_images", //upload files to the specified folder
    // tags: ["users", "profile"], //add the given tags to the uploaded files
    // context: {alt: "user_uploaded"}, //add the given context data to the uploaded files
    // clientAllowedFormats: ["images"], //restrict uploading to image files only
    // maxImageFileSize: 2000000,  //restrict file size to less than 2MB
    // maxImageWidth: 2000, //Scales the image down to a width of 2000 pixels before uploading
    // theme: "purple", //change to a purple theme
  });
```

Now, we will create Cloudinary instance cld with our “cloudName” state variable. This constructor was imported from the @cloudinary/url-gen package.<br>
```
  const cld = new Cloudinary({
    cloud: {
      cloudName
    }
  });
```

We will create a Cloudinary image object “myImage”, which will allow us to generate the Cloudinary image with the publicId to properly identify the image in our Cloudinary database.<br>
```
  const myImage = cld.image(publicId);
```

Lastly, we will return the basic page, which will render the CloudinaryUploadWidget component from before with our uwConfig and setPublicId props/parameters.<br>

This AdvancedImage component will be used to show the uploaded image. It’ll use the configurations that we defined before in myImage. Optionally, we also can add plugins “responsive()” and “placeholder()” for resizing and placeholder images.<br>
```
  return (
    <div className="App">
      <h3>Cloudinary Upload Widget Example</h3>
      <CloudinaryUploadWidget uwConfig={uwConfig} setPublicId={setPublicId} />
      <div style={{ width: "800px" }}>
        <AdvancedImage
          style={{ maxWidth: "100%" }}
          cldImg={myImage}
          plugins={[responsive(), placeholder()]}
        />
      </div>
    </div>
  );
}
```

Good job, we are now complete and have a React app that we can run!<br>

Run the application with “npm run start”.<br>
![Upload Button](/public/cloudinary-app-imgs/cloudinary_app7.png)

Click the upload button with our shiny new widget, choose any image, and click done.<br>
![Uploading](/public/cloudinary-app-imgs/cloudinary_app9.png)

Now, you will see the properly uploaded image, which will be stored in your Cloudinary account. This image will have a link accessible to the public.<br>
![Uploaded Image](/public/cloudinary-app-imgs/cloudinary_app8.png)


