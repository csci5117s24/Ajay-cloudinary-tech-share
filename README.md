# Cloudinary Tech Demo
![Cloudinary_Logo](/public/cloudinary-logo.png)

## Introduction
### Why Cloud Storage?
Many companies have been switching to this form of data storage due to its scalability, flexibility, cost-effectiveness, and enhanced security features compared to their traditional physical storage counterparts. Since they often offer their services on a pay-per-use basis, developers have a range of options depending on which cloud service serves their needs and budget.

### Why Cloudinary?
For Project 1, we felt that managing image storage and retrieval within our own database was quite tedious. As a result, we wanted to find a solution that would minimize our manual workload. 

Cloudinary offers a [free "Programmable Media" plan](https://cloudinary.com/pricing) that offers many useful features. With this plan, users are alloted 25 credits each month for their storage. The credit breakdown is further elaborated on [here](https://cloudinary.com/documentation/developer_onboarding_faq_credits). Specifically for images, this essentially provides 25GB of image storage each month. It's important to note that the free plan does have [additional limitations](https://cloudinary.com/pricing/compare-plans), specifically on the file size and number of requests, but we found that these limitations are reasonable for a project of our scale.

The features of Cloudinary that stand out to us is that they provide developers with a widget that allows users to upload images from a variety of platforms, processes the file, and returns a link to the image for the developers to store. This image can now be referenced through a URL.

# Ensure installation of cloudinary: 
 **To install run this command in root directory: npm i @cloudinary/url-gen @cloudinary/react** 

## Cloudinary Upload Widget
For simplicity and best code-reuse, we will make a CloudinaryUploadWidget component called CloudinaryUploadWidget.jsx. We will be able to use this widget throughout our application whenever we would like users to upload an image.

First, we import all necessary components from the React library. These will be used to manage state and effects in our CloudinaryUploadWidget component.
[Import Statement](/public/cloudinary-import.PNG)

Then, we will create a context in order to manage our script loading state. This way we can pass data through the component without having to pass our props down manually.
[createContext](/public/cloudinary-context.PNG)

Now, we will start our CloudinaryUploadWidget function with two parameters, uwConfig and setPublicId.
[Widget Function](/public/cloudinary-config.PNG)

We will also use  state variables for tracking whether the Cloudinary script has been uploaded.
[Widget State](/public/cloudinary-state.PNG)

Next, we will use useEffect in order to properly load the Cloudinary script based on the “loaded” state. Within this effect, we will look at the loaded state variable to see if the scripts have been loaded, if not, a script element is created and added to the document.
[Use Effect](/public/cloudinary-useEffect.PNG)

Next, we will create an initializeCloudinaryWidget function to initialize the CloudinaryUploadWidget when the script has been successfully loaded. 
[Script Check](/public/cloudinary-script.PNG)

Once the script has been loaded, we use the command window.cloudinary.createUploadWidget, with our preset uwConfig. Whenever an image is successfully uploaded, publicId will be set and we will have access to the image information.
[Initialize Widget](/public/cloudinary-init.PNG)

Then, we will add an event listener to the widget to ensure the Cloudinary upload widget is properly opened on button click for asset uploads.
[Check Widget](/public/cloudinary-check.PNG)

Lastly, we return the button upload_widget, which will initialize the Cloudinary Widget once pressed, and handle publicID once the assets are properly uploaded.
[Widget Button](/public/cloudinary-button.PNG)

At this point, we now have a React component, CloudinaryUploadWidget to create and manage uploaded assets to a user's Cloudinary account.

