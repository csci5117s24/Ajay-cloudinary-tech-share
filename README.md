# Cloudinary Tech Demo
![Cloudinary_Logo](/public/cloudinary-logo.png)

## Why Cloud Storage?
Many companies have been switching to this form of data storage due to its scalability, flexibility, cost-effectiveness, and enhanced security features compared to their traditional physical storage counterparts. Since they often offer their services on a pay-per-use basis, developers have a range of options depending on which cloud service serves their needs and budget.

## Why Cloudinary?
For Project 1, we felt that managing image storage and retrieval within our own database was quite tedious. As a result, we wanted to find a solution that would minimize our manual workload. 
Cloudinary offers a [free "Programmable Media" plan](https://cloudinary.com/pricing) that offers many useful features. With this plan, users are alloted 25 credits each month for their storage. The credit breakdown is further elaborated on [here](https://cloudinary.com/documentation/developer_onboarding_faq_credits). Specifically for images, this essentially provides 25GB of image storage each month. It's important to note that the free plan does have [additional limiations](https://cloudinary.com/pricing/compare-plans), specifically on the file size and number of requests, but we found that these limitations are reasonable for a project of our scale.. 
The features of Cloudinary that stand out to us is that they provide developers with a widget that allows users to upload images from a variety of platforms, processes the file, and returns a link to the image for the developers to store. This image can now be referenced through a URL.

# Ensure installation of cloudinary: 
 **To install run this command in root directory: npm i @cloudinary/url-gen @cloudinary/react** 
