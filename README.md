# Google Drive Disable Downloading

This Google Apps Script is designed to update all files in your Google Drive to disable the options to download, print, or copy for viewers and commenters.

## Getting Started

These instructions will help you set up this script for use in your Google Drive.

### Prerequisites

To use this script, you need to enable the Google Drive API in your Google Apps Script project:

1. Open the Apps Script project.
2. At the left, click Editor code.
3. At the left, next to Services, click Add a service add.
4. Select an 'Drive API' and click Add.

### Script

Here is the script:

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

### Usage
You can run this script in the Apps Script editor.

Please remember that you'll need to have edit permissions for all files in the folder for this script to work, and changes made through the Drive API can take up to 2 minutes to propagate and become visible in the Google Drive interface.

### Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.
