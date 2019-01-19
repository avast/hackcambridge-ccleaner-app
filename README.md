### Getting Started

Below is a list of files included in this repository and a short description of what they do:

|File|Description|
|:--|:--|
|CCleaner.dat|License file for CCleaner|
|CCleaner.exe|CCleaner Application (32-bit)|
|CCleaner64.exe|CCleaner Application (64-bit)|
|ccleaner.ini|Portable settings file for CCleaner configuration options|
|asb-cc-installpath-sample.reg|Sample registry file for linking ASB <-> CCleaner|
|clean-test.vbs|Sample Visual Basic script for cleaning actions|
|portable.dat|Empty file that enables CCleaner to run anywhere|
|report-ccleaner-sample.json|Sample cleaning report dictionary in JSON format|
|winapp1.ini|CCleaner configuration file for ASB analysis|
|winapp2.ini|CCleaner configuration file for ASB cleaning|
|winsys1.ini|Empty file to hide unrelated applications in UI|

### Cleaning Avast Secure Browser (ASB)

For CCleaner to clean Avast Secure Browser, the application can utilise the command line arguments (CLAs) made available in the custom build of the browser. When calling `AvastBrowser.exe`, use the CLAs to run cleaning commands (provided by browser extension APIs under the hood) to report results into a file with JSON format.

Here are the API reference details:

`--clean-browsing-data=`*(required, non-empty)* - a comma separated list of data types to clean. Possible values are: 

- `appcache`
- `cache`
- `cacheStorage`
- `cookies`
- `downloads`
- `fileSystems`
- `formData`
- `history`
- `indexedDB`
- `localStorage`
- `serverBoundCertificates`
- `pluginData`
- `passwords`
- `serviceWorkers`
- `webSQL`

`--clean-browsing-data-origin=`*(optional)* - a comma separated list of origin types to clean. 

Possible values are:

- `unprotectedWeb`
- `protectedWeb`
- `extension`

`--clean-browsing-data-timerange=[start]-[end]`*(optional)* specifies the timestamps to clean between. This timestamp is formatted as the number of milliseconds since the Unix epoch. If `[end]` is missed then it removes until "now".

`--clean-browsing-data-report=`*(optional)* - path of the file to write the cleaning report data to. The format is a dictionary with data types that were requested to be cleaned. Each data type has a `count` (i.e. total number of entries) and an `entries` array of objects. Inside the `entries` array, there is `path` and `size` information. The `path` may be a URL or some other identifier (e.g. `profile:guid` for form data). The `size` information is represented in bytes and is non-zero only for some data types (e.g. `cache`).

Sample command:
`AvastBrowser.exe --clean-browsing-data=cache,cookies,formData,history,passwords --clean-browsing-data-report=C:\ccleaner-report.json`

*Important Note:* for more information about cleaning categories and origin types supported by the Avast Secure Browser, please visit the Chrome Browsing Data developer reference page [here](https://developer.chrome.com/extensions/browsingData).

### Reporting Results

To report the browser cleaning results back to CCleaner, you can pass a JSON dictionary as follows:

Sample command:
`CCleaner.exe /report "report-ccleaner-sample.json"`

### Connecting CCleaner & ASB

The provided sample registry key file will enable the browser to load CCleaner and pass cleaning report data back to it. You can verify this by visiting the browser's [Security & Privacy Center](secure://security-privacy-center/) page and observing that the 'Privacy Cleaner' tile has updated to 'CCleaner'.

### Useful Links

- [Windows Script Host](https://docs.microsoft.com/en-us/previous-versions/windows/internet-explorer/ie-developer/windows-scripting/9bbdkx3k(v%3dvs.84)) - provides information about Windows Script Host along with a reference section that documents the object model.

- [CCleaner Command Line Parameters](https://www.ccleaner.com/docs/ccleaner/advanced-usage/command-line-parameters) - you can use command-line parameters to change how CCleaner runs.
