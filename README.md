# Cloudinary Tech Demo
![Cloudinary_Logo](/public/cloudinary-logo.png)

## Introduction
### Why Cloud Storage?
Many companies have been switching to this form of data storage due to its scalability, flexibility, cost-effectiveness, and enhanced security features compared to their traditional physical storage counterparts. Since they often offer their services on a pay-per-use basis, developers have a range of options depending on which cloud service serves their needs and budget.

### Why Cloudinary?
For Project 1, we felt that managing image storage and retrieval within our own database was quite tedious. As a result, we wanted to find a solution that would minimize our manual workload. 

Cloudinary offers a [free "Programmable Media" plan](https://cloudinary.com/pricing) that offers many useful features. With this plan, users are alloted 25 credits each month for their storage. The credit breakdown is further elaborated on [here](https://cloudinary.com/documentation/developer_onboarding_faq_credits). Specifically for images, this essentially provides 25GB of image storage each month. It's important to note that the free plan does have [additional limitations](https://cloudinary.com/pricing/compare-plans), specifically on the file size and number of requests, but we found that these limitations are reasonable for a project of our scale.

The features of Cloudinary that stand out to us is that they provide developers with a widget that allows users to upload images from a variety of platforms, processes the file, and returns a link to the image for the developers to store. This image can now be referenced through a URL.

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
For simplicity and best code-reuse, we will make a CloudinaryUploadWidget component called CloudinaryUploadWidget.jsx. We will be able to use this widget throughout our application whenever we would like users to upload an image.

First, we import all necessary components from the React library. These will be used to manage state and effects in our CloudinaryUploadWidget component.<br>
![Import Statement](/public/widget-imgs/cloudinary-import.PNG)

Then, we will create a context in order to manage our script loading state. This way we can pass data through the component without having to pass our props down manually.<br>
![createContext](/public/widget-imgs/cloudinary-context.PNG)

Now, we will start our CloudinaryUploadWidget function with two parameters, uwConfig and setPublicId.<br>
![Widget Function](/public/widget-imgs/cloudinary-config.PNG)

We will also use  state variables for tracking whether the Cloudinary script has been uploaded.<br>
![Widget State](/public/widget-imgs/cloudinary-state.PNG)

Next, we will use useEffect in order to properly load the Cloudinary script based on the “loaded” state. Within this effect, we will look at the loaded state variable to see if the scripts have been loaded, if not, a script element is created and added to the document.<br>
![Use Effect](/public/widget-imgs/cloudinary-useEffect.PNG)

Next, we will create an initializeCloudinaryWidget function to initialize the CloudinaryUploadWidget when the script has been successfully loaded. <br>
![Script Check](/public/widget-imgs/cloudinary-script.PNG)

Once the script has been loaded, we use the command window.cloudinary.createUploadWidget, with our preset uwConfig. Whenever an image is successfully uploaded, publicId will be set and we will have access to the image information.<br>
![Initialize Widget](/public/widget-imgs/cloudinary-init.PNG)

Then, we will add an event listener to the widget to ensure the Cloudinary upload widget is properly opened on button click for asset uploads.<br>
![Check Widget](/public/widget-imgs/cloudinary-check.PNG)

Lastly, we return the button upload_widget, which will initialize the Cloudinary Widget once pressed, and handle publicID once the assets are properly uploaded.<br>
![Widget Button](/public/widget-imgs/cloudinary-button.PNG)

At this point, we now have a React component, CloudinaryUploadWidget to create and manage uploaded assets to a user's Cloudinary account.

