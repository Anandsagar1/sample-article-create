# sample-article-create
This is a sample of a custom web application for creating new "Article" content items with images in IBM Watson Content Hub (WCH). It also includes an example of how to update an existing content item which is in ready state.

This sample shows:
* Authenticating to the Watson Content Hub and calling APIs that require authentication.
* Using the authoring services for resources, assets, and content to upload a resource, create an asset, and create an "Article" content item that includes the image.

This sample can be used to create new articles that will be displayed in this earlier sample: https://github.com/ibm-wch/sample-article-carousel

This screenshot shows the completed form for a new Article:
![Alt text](/docs/create-article-screenshot.jpg?raw=true "Sample screenshot")

###Running the sample

#### 1. Download the files

Download the application files (html, js, and css) from the 'public' folder into any folder on your workstation.

#### 2. Update the baseTenantAPIURL

The baseTenantAPIURL variable in app.js must be set for your tenant. In the IBM Watson Content Hub user interface, open the user menu from the top navigation bar, then select "Hub information". The pop-up window shows your API URL, host and content hub ID for your Watson Content Hub tenant. Use this information to update the value of the baseTenantAPIURL variable in public/app.js, in the form https://{host}/api/{content hub tenant id}. For example it might look something like this:

const baseTenantAPIURL = "https://my12.digitalexperience.ibm.com/api/12345678-9abc-def0-1234-56789abcdef0";


#### 3. Create the "Article" content type

This application uses an "Article" content type that must be created for your tenant prior to running the sample the first time.

Follow the instructions at the [sample-article-content](https://github.com/ibm-wch/sample-article-content) repository, to download and push the sample article type and associated authoring artifacts, for your content hub tenant.

#### 4. Enable CORS support for your tenant

For this scenario you will need to enable CORS support for your tenant. To control the CORS enablement for Watson Content Hub, go to Hub set up -> General settings -> Security tab. After adding your domain (or "*" for any domain), be sure to click the Save button at the top right of the screen.

#### 5. Load index.html in a browser to test creating a new "Article" content type item

You can do this right from the file system in Firefox, Chrome, or Safari browsers. Alternatively you can make the files available on any web server and open index.html in a browser using your web server URL.

When prompted for a username and password,  you must use an IBM id for a user associated with the Watson Content Hub tenant specified in the above baseTenantUrl.

### 6. Similarly load index_update.html in a browser to test updating the content item

You will need to enter the content item id of the content item to be updated. This can be found in the API information section of the particular content item in Watson Content Hub console 


### Implementation notes for creating a content item

#### Specifying the content type when creating a content item

The API for creating a new content items requires the ID of a content type rather than the name of the content type. In this sample the ID of the "article" type is determined by doing a search for a content type of name "article". Alternatively, if you are working with a content type that you have defined, you can code the ID directly in your application. The ID is guaranteed not to change once a content type is created.

#### Creating an image asset and referencing it in a content item
Creating a new image (or other file) asset in WCH is a two-step process: first the file is uploaded to the resources service, and then the returned ID for the resource is used to create an asset. In this sample the HTML5 File object is used with XMLHttpRequest to upload the file.

Once the image asset is created, the returned JSON can be used in building the element data that is used when creating a content item.

#### Building the elements data for the new content item
In this example, there is a hard-coded "emptyElements" structure that matches the fields and element types that the Article content type expects.

#### Using the helper functions for the steps in creating the content item
The individual API calls used in this example are broken out into separate reusable functions so that you could use them in different ways.

In this sample all of the following calls are used:
- Login, with wchLogin
- Upload resource, with wchCreateResource
- Create asset from resource ID, with wchCreateAssetFromResource
- Search for content type "article," with wchSearch
- Create content item, with wchCreateContentItem

If, for example, you just want to create a content item with only text fields using a known content type ID, you would just call:
- wchLogin
- wchCreateContentItem

And if you just want to upload a file and create a new asset you would call:
- wchLogin
- wchCreateResource
- wchCreateAssetFromResource

### Implementation notes for updating a content item

#### Create a draft version of the content item to be updated via WCH API
The API for creating a draft version of a content item requires the ID of the content item. In this sample the ID of the content item is entered as the text input. The content item id can be obtained from the Watson Content Hub console in the API information section.

#### Create an image asset and update the image reference in the content item
Creating an image asset is same as what is explained above while creating the content item. Except the created image reference is then updated in the draft content item created.

#### Update the content item
Update the values  and change the content item status back to ready. Following are the helper functions used – 
- Login, with wchLogin
- Upload resource, with wchCreateResource
- Create asset from resource ID, with wchCreateAssetFromResource
- Create a draft version of the content item, update it and change it back to ready state, with wchUpdateContentItem

#### Note – Creating a draft version of the content item is only required when trying to update the content item in ready state. Creating a draft verion of the content item is not required while updating the content item in draft state.

### Resources

API Explorer reference documentation: https://developer.ibm.com/api/view/id-618

Watson Content Hub developer center: https://developer.ibm.com/wch/

Watson Content Hub forum: https://developer.ibm.com/answers/smartspace/wch/
