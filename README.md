# Google Drive: Disable Download, Print, and Copy Automation Script

This repository contains Google Apps Script(s) that help manage access permissions across files in your Google Drive. Specifically, these scripts remove the options for viewers and commenters to download, print, or copy the files.

## Setup and Prerequisites

Before running these scripts, ensure you have enabled the Google Drive API for your Apps Script project. Here are the steps to enable it:

1. Open the Apps Script project.
2. On the left, click on 'Editor code'.
3. Next to 'Services', click on 'Add a service'.
4. From the list, select 'Drive API' and click on 'Add'.

## Scripts

This repository provides two scripts:

### 1. Apply Permission Changes to All Files
This script applies the permission changes to all files in your Google Drive, traversing all folders and sub-folders.

```javascript
function disableDownloadPrintCopyForAll() {
  // Start from the root folder of Google Drive
  var rootFolder = DriveApp.getRootFolder();

  // Traverse and update all files and folders starting from the root
  traverseAndUpdateFiles(rootFolder);
}

function traverseAndUpdateFiles(folder) {
  var files = folder.getFiles();

  while (files.hasNext()) {
    var file = files.next();
    var fileId = file.getId();

    // Change the file's settings so that viewers and commenters can't download, print, or copy
    var resource = {
      'copyRequiresWriterPermission': true
    };

    Drive.Files.update(resource, fileId);
  }

  // Recursively call this function for each sub-folder
  var subfolders = folder.getFolders();
  
  while (subfolders.hasNext()) {
    var subfolder = subfolders.next();
    traverseAndUpdateFiles(subfolder);
  }
}
```


### 2. Apply Permission Changes to a Specific Root-Level Folder
This script applies the permission changes to a specific folder at the root level of your Google Drive and its sub-folders. In this example, we assume the folder is named 'Projects'.


```javascript
function disableDownloadPrintCopyForFolder() {
  // Find the 'Projects' folder at the root level of Google Drive
  var folders = DriveApp.getRootFolder().getFolders();
  var targetFolder = null;

  while (folders.hasNext()) {
    var folder = folders.next();

    if (folder.getName() === 'Projects') {
      targetFolder = folder;
      break;
    }
  }

  // If the 'Projects' folder was found, change the settings for all files within it and its sub-folders
  if (targetFolder != null) {
    traverseAndUpdateFiles(targetFolder);
  } else {
    Logger.log("The 'Projects' folder was not found at the root level of your Google Drive.");
  }
}

function traverseAndUpdateFiles(folder) {
  var files = folder.getFiles();

  while (files.hasNext()) {
    var file = files.next();
    var fileId = file.getId();

    // Change the file's settings so that viewers and commenters can't download, print, or copy
    var resource = {
      'copyRequiresWriterPermission': true
    };

    Drive.Files.update(resource, fileId);
  }

  // Recursively call this function for each sub-folder
  var subfolders = folder.getFolders();
  
  while (subfolders.hasNext()) {
    var subfolder = subfolders.next();
    traverseAndUpdateFiles(subfolder);
  }
}
```


### Usage
You can run this script in the Apps Script editor.

Please remember that you'll need to have edit permissions for all files in the folder for this script to work, and changes made through the Drive API can take up to 2 minutes to propagate and become visible in the Google Drive interface.

### Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.
