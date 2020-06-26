# Rich Edit for ASP.NET Core How to integrate a control into a Blazor server application

## Requirements
- To use the RichEdit control in an Blazor application, you need to have a [Universal, DXperience, or ASP.NET subscription](https://www.devexpress.com/buy/net/).
- Versions of the devexpress npm packages should be identical (their major and minor versions should be the same).
 
This example illustrates a possible way of integrating a client part of ASP.NET Core Rich Edit into an ASP.NET Web Forms application. This can be done as follows:

1. Create a new Blazor application using recommendations from the following topic: [Get started with ASP.NET Core Blazor](https://docs.microsoft.com/en-us/aspnet/core/blazor/get-started?view=aspnetcore-3.1&tabs=visual-studio)
2. Install necessary NPM packages
 
The **devexpress-richedit** npm package references **devextreme** as [peerDependencies](https://docs.npmjs.com/files/package.json#peerdependencies). The peerDependencies packages should be installed manually. This allows developers to control a version of the peerDependencies packages and guarantees that the package is installed once.
 
Install a RichEdit package with required peerDependencies:
 
* Create npm Configuration File if the ```package.json``` file unexist by the command ```npm init -y```
* Add the following dependencies to this file:
```
{
    ...
  "dependencies": {
    "devextreme": "<:xx.x.x:>",
    "devexpress-richedit": "<:xx.x.x:>"
  }
}
```
* Call ```npm i```.
You can find all the libraries in the **node_modules** folder after the installation is completed.
 
3. Create a rich edit bundle using recommendations from this help topic: [Create a RichEdit Bundle](https://docs.devexpress.com/AspNetCore/401721/office-inspired-controls/get-started/richedit-bundle)

```bash
cd node_modules/devexpress-richedit
npm i --save-dev
npm run build-custom
cd ../..
```
 
3. Copy rich edit scripts
* Copy the bundled script from ```node_modules/devexpress-richedit/dist/custom/dx.richedit.min.js``` to ```wwwroot/js```
* Copy the bundled css resources from ```node_modules/devexpress-richedit/dist/custom/dx.richedit.css``` and icons from ```node_modules/devexpress-richedit/dist/custom/icons``` to ```wwwroot/css```
 
4. Register scripts in the <head> tag of ```Pages/_Host.cshtml```
 
```
<link href="css/dx.richedit.css" rel="stylesheet" />
<script src="js/dx.richedit.min.js"></script>
```
 
5. Create a JavaScript rich edit initializing function. For this, create the ```wwwroot/js/richedit-creator.js``` file and place the following content:
 
```javascript
function createRichEdit(documentAsBase64) {
    const options = DevExpress.RichEdit.createOptions();
    options.confirmOnLosingChanges.enabled = false;
    options.exportUrl = 'api/RichEdit/SaveDocument';
    options.width = '1400px';
    options.height = '900px';
    var richElement = document.getElementById("rich-container");
    window.richEdit = DevExpress.RichEdit.create(richElement, options);
    if (documentAsBase64)
        window.richEdit.openDocument(documentAsBase64, "DocumentName", DevExpress.RichEdit.DocumentFormat.Rtf);
}
```
 
Register the created script in the <head> tag of ```Pages/_Host.cshtml```
 
```
<script src="js/richedit-creator.js"></script>
```

<!-- default file list -->
*Files to look at*:

* [_Host.cshtml](./CS/Pages/_Host.cshtml)
* [Index.razor](./CS/Pages/Index.razor)
* [richedit-creator.js](./CS/wwwroot/js/richedit-creator.js)
* [RichEditController.cs](./CS/Controllers/RichEditController.cs)
<!-- default file list end -->