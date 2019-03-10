https://helpcenter.onlyoffice.com/desktop/documents/windows/compile-desktop-windows.aspx

# Windows Build

## Installing software necessary to build Desktop Editors

First you will need to install the software necessary to compile Desktop Editors source code. The following programs must be installed:

* 7z
* Git for Windows
* svn (command line)
* Microsoft Visual Studio 2015 Community
* Microsoft Windows SDK 10 (with debug)
* Qt 5.8
* PowerShell 4
* Node.js

After everything is installed, you can continue to the next step.

## Getting and building Desktop Editors source code

### Getting the source code

Run the Windows command line tool, create the folder for the project and checkout the project from the GitHub:

```
mkdir C:\GIT\
cd C:\GIT\
git clone --recursive https://github.com/ONLYOFFICE/DesktopEditors.git
```

### Build the third-party core components

A part of the compilation of a 3rd party component (V8) needs to execute external PowerShell Script, that is probably forbidden by default on your PC.

To allow the excution of external PowerShell Script, run PowerShell with administrator privileges and run (see [https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6):

```
Set-ExecutionPolicy Unrestricted
```

Now fetch and build the third-party core components. To do that run the following .bat file:

```
C:\GIT\DesktopEditors\core\Common\3dParty\make.bat
```

Once finished, return to security by running in a PowerShell with administrator privileges:

```
Set-ExecutionPolicy Undefined
```

### Build Qt components

Now build the Qt-based components and libraries using the Qt compiler installed. This is done using the following Qt project files:


* `C:\GIT\DesktopEditors\core\Common\3dParty\cryptopp\project`
* `C:\GIT\DesktopEditors\core\Common\kernel.pro`
* `C:\GIT\DesktopEditors\core\DesktopEditor\graphics\pro\graphics.pro`
* `C:\GIT\DesktopEditors\core\UnicodeConverter\UnicodeConverter.pro`
* `C:\GIT\DesktopEditors\core\PdfWriter\PdfWriter.pro`
* `C:\GIT\DesktopEditors\core\DesktopEditor\doctrenderer\doctrenderer.pro`
  * For this one, I had to fix `DesktopEditor/doctrenderer/nativecontrol.h` to make it compile
* `C:\GIT\DesktopEditors\core\HtmlRenderer\htmlrenderer.pro`
* `C:\GIT\DesktopEditors\core\PdfReader\PdfReader.pro`
* `C:\GIT\DesktopEditors\core\DjVuFile\DjVuFile.pro`
* `C:\GIT\DesktopEditors\core\XpsFile\XpsFile.pro`
* `C:\GIT\DesktopEditors\core\HtmlFile\HtmlFile.pro`
* `C:\GIT\DesktopEditors\core\X2tConverter\build\Qt\X2tSLN.pro`
* `C:\GIT\DesktopEditors\desktop-sdk\HtmlFile\Internal\Internal.pro`
* `C:\GIT\DesktopEditors\core\DesktopEditor\hunspell-1.3.3\src\qt\hunspell.pro`
* `C:\GIT\DesktopEditors\core\DesktopEditor\xmlsec\src\ooxmlsignature.pro`

### Build core and application

Now you can build the desktop application core components and the application itself. The desktop core is built with the Qt project file:

* `C:\GIT\DesktopEditors\desktop-sdk\ChromiumBasedEditors\lib\AscDocumentsCore_win.pro`

After that copy the cef and icu libraries to the core/build folder:

|  Source files/folders | Destination files/folders  | 
|---|---|
|  `C:\GIT\DesktopEditors\core\Common\3dParty\cef\win_64\build` | `C:\GIT\DesktopEditors\core\build\cef\win_64`  |  
|  `C:\GIT\DesktopEditors\core\Common\3dParty\icu\win_64\build` | `C:\GIT\DesktopEditors\core\build\bin\icu\win_64`  | 


Then build the application using the Qt compiler project file:

* `C:\GIT\DesktopEditors\desktop-apps\win-linux\ASCDocumentEditor.pro`


### Build sdkjs

The next component necessary for Desktop Editors correct work is sdkjs. It is built the following way:

Go to the sdkjs build directory and create the following folders:

```
cd C:\GIT\DesktopEditors\sdkjs\build
mkdir C:\GIT\DesktopEditors\core\build\jsdesktop
mkdir C:\GIT\DesktopEditors\core\build\jsdesktop\sdkjs
mkdir C:\GIT\DesktopEditors\core\build\jsdesktop\sdkjs\slide
mkdir C:\GIT\DesktopEditors\core\build\jsdesktop\sdkjs\cell
mkdir C:\GIT\DesktopEditors\core\build\jsdesktop\sdkjs\word
mkdir C:\GIT\DesktopEditors\core\build\jsdesktop\sdkjs\common
mkdir C:\GIT\DesktopEditors\core\build\jsdesktop\sdkjs\common\Native
mkdir C:\GIT\DesktopEditors\core-ext\desktop-sdk-wrapper
mkdir C:\GIT\DesktopEditors\core-ext\desktop-sdk-wrapper\test
mkdir C:\GIT\DesktopEditors\core-ext\desktop-sdk-wrapper\test\build
mkdir C:\GIT\DesktopEditors\core-ext\desktop-sdk-wrapper\test\build\win_64
mkdir C:\GIT\DesktopEditors\core-ext\desktop-sdk-wrapper\test\build\win_64\Debug
mkdir C:\GIT\DesktopEditors\core-ext\desktop-sdk-wrapper\test\build\win_64\Debug\editors
mkdir C:\GIT\DesktopEditors\core-ext\desktop-sdk-wrapper\test\build\win_64\Debug\editors\sdkjs\word
mkdir C:\GIT\DesktopEditors\core-ext\desktop-sdk-wrapper\test\build\win_64\Debug\editors\sdkjs\slide
mkdir C:\GIT\DesktopEditors\core-ext\desktop-sdk-wrapper\test\build\win_64\Debug\editors\sdkjs\cell
mkdir C:\GIT\DesktopEditors\core-ext\desktop-sdk-wrapper\test\build\win_64\Debug\editors\sdkjs\common\Native
```


Compile:

```
./build-desktop.bat
```

And copy the files:

|  Source files/folders | Destination files/folders  | 
|---|---|
|  `C:\GIT\DesktopEditors\sdkjs\common\Native\jquery_native.js` |  `C:\GIT\DesktopEditors\core\build\jsdesktop\sdkjs\common\Native`  | 


### Build web-apps component

Now the web-apps component is built. To do that switch to the folder:

```
cd C:\GIT\DesktopEditors\web-apps\build
```

And run the commands:

```
npm install
grunt
```

### Build the start page

Run the commands:

```
cd C:\GIT\DesktopEditors\desktop-apps\common\loginpage\build
npm install
cd C:\GIT\DesktopEditors\desktop-apps\common\loginpage\build\node_modules\grunt-contrib-uglify
```

Edit the package.json file so that it contained the following line:

```
"dependencies": {
  ...
  "uglify-js" : "git+https://github.com/mishoo/UglifyJS2.git#harmony",
  ...
}
```

Now update the npm information while staying in the node_modules/grunt-contrib-uglify directory:

```
npm update
```

And build Desktop Editors start page:

```
cd C:\GIT\DesktopEditors\desktop-apps\common\loginpage\build
grunt --force
```

## Copying files to the Desktop Editors application folder

Now you can copy all the files necessary for the portable version of Desktop Editors. First create a folder where all the files will be stored and go to it:

```
mkdir C:\GIT\DesktopEditors\app
cd C:\GIT\DesktopEditors\app
```

Now the following files and folders need to be copied:

|  Source files/folders | Destination files/folders  | 
|---|---|
| `C:\Qt\5.8\msvc2015_64\plugins\bearer` |  `./bearer/`  | 
| `C:\Qt\5.8\msvc2015_64\plugins\imageformats` |  `./imageformats/` | 
| `C:\Qt\5.8\msvc2015_64\plugins\platforms` |  `./platforms/`  | 
| `C:\Qt\5.8\msvc2015_64\plugins\printsupport` |  `./printsupport/`  |


> Please note that only release Qt DLLs must be copied.

|  Source files/folders | Destination files/folders  | 
|---|---|
| `C:\Qt\5.8\msvc2015_64\bin\Qt5Core.dll`| `./` |
| `C:\Qt\5.8\msvc2015_64\bin\Qt5Gui.dll`| `./` |
| `C:\Qt\5.8\msvc2015_64\bin\Qt5PrintSupport.dll`| `./` |
| `C:\Qt\5.8\msvc2015_64\bin\Qt5Svg.dll`| `./` |
| `C:\Qt\5.8\msvc2015_64\bin\Qt5Widgets.dll`| `./` |
| `C:\GIT\DesktopEditors\dictionaries`| `./dictionaries/` |
| `C:\GIT\DesktopEditors\desktop-apps\common\package\fonts`| `./fonts/` |
| `C:\GIT\DesktopEditors\core\build\cef\win_64`| `./` |
| `C:\GIT\DesktopEditors\core\build\lib\win_64\HtmlFileInternal.exe`| `./` |



Now create the converter folder:

```
mkdir ./converter/
```

And copy the files:

|  Source files/folders | Destination files/folders  | 
|---|---|
| `C:\GIT\DesktopEditors\core\build\lib\win_64\kernel.dll`| `./converter/` |
| `C:\GIT\DesktopEditors\core\build\lib\win_64\graphics.dll`| `./converter/` |
| `C:\GIT\DesktopEditors\core\build\lib\win_64\DjVuFile.dll`| `./converter/` |
| `C:\GIT\DesktopEditors\core\build\lib\win_64\HtmlFile.dll`| `./converter/` |
| `C:\GIT\DesktopEditors\core\build\lib\win_64\HtmlRenderer.dll`| `./converter/` |
| `C:\GIT\DesktopEditors\core\build\lib\win_64\PdfReader.dll`| `./converter/` |
| `C:\GIT\DesktopEditors\core\build\lib\win_64\PdfWriter.dll`| `./converter/` |
| `C:\GIT\DesktopEditors\core\build\lib\win_64\UnicodeConverter.dll`| `./converter/` |
| `C:\GIT\DesktopEditors\core\build\lib\win_64\XpsFile.dll`| `./converter/` |
| `C:\GIT\DesktopEditors\core\build\lib\win_64\doctrenderer.dll`| `./converter/` |
| `C:\GIT\DesktopEditors\core\build\bin\win_64\x2t.exe`| `./converter/` |
| `C:\GIT\DesktopEditors\core\build\bin\icu\win_64\icudt58.dll`| `./converter/` |
| `C:\GIT\DesktopEditors\core\build\bin\icu\win_64\icuuc58.dll`| `./converter/` |
| `C:\GIT\DesktopEditors\core\Common\3dParty\v8\v8\third_party\icu\windows\icudt.dll`| `./converter/` |
| `C:\GIT\package`| `./converter/` |


Copy the desktop editors files:

|  Source files/folders | Destination files/folders  | 
|---|---|
| `C:\GIT\DesktopEditors\core\build\lib\win_64\ascdocumentscore.dll`| `./` |
| `C:\GIT\DesktopEditors\core\build\lib\win_64\hunspell.dll`| `./` |
| `C:\GIT\DesktopEditors\core\build\lib\win_64\ooxmlsignature.dll`| `./` |
| `C:\GIT\DesktopEditors\desktop-apps\build-ASCDocumentEditor-Desktop_Qt_5_8_0_MSVC2015_64bit-Release\DesktopEditors.exe`| `./` |
| `C:\GIT\DesktopEditors\desktop-apps\win-linux\res\icons\desktopeditors.ico` | `./app.ico` |

Add the `C:\GIT\DesktopEditors\app\converter` path to the PATH environment variable.

Then create the js engine folder:

```
mkdir ./editors
```

And copy the files:

|  Source files/folders | Destination files/folders  | 
|---|---|
|`C:\GIT\DesktopEditors\web-apps\deploy\sdkjs`|`./editors/sdkjs`|
|`C:\GIT\DesktopEditors\web-apps\deploy\web-apps`|`./editors/web-apps`|

Finally copy the start page:

|  Source files/folders | Destination files/folders  | 
|---|---|
|`C:\GIT\DesktopEditors\desktop-apps\common\loginpage\deploy\index.html`|`./`| 


Now you can run Desktop Editors using the `DesktopEditors.exe` file.

# Orignal Readme


[![License](https://img.shields.io/badge/License-GNU%20AGPL%20V3-green.svg?style=flat)](https://www.gnu.org/licenses/agpl-3.0.en.html) ![Release](https://img.shields.io/badge/Release-v4.4-blue.svg?style=flat) ![Platforms Windows | macOS | Linux](https://img.shields.io/badge/Platforms-Window%20%7C%20macOS%20%7C%20Linux-lightgrey.svg?style=flat)

## Overview

ONLYOFFICE Desktop Editors is a free office suite that combines text, spreadsheet and presentation editors allowing to create, view and edit documents stored on your Windows/Linux PC or Mac without an Internet connection. It is fully compatible with Office Open XML formats: .docx, .xlsx, .pptx.

## Components

ONLYOFFICE Desktop Editors contain the following components:

* [desktop-apps](https://github.com/ONLYOFFICE/desktop-apps "desktop-apps") - the frontend for ONLYOFFICE Desktop Editors which is used to build the program interface for the operating system selected.
* [desktop-sdk](https://github.com/ONLYOFFICE/desktop-sdk "desktop-sdk") - SDK which is a core part of ONLYOFFICE Desktop Editors.
* [core](https://github.com/ONLYOFFICE/core "core") - server core components for [ONLYOFFICE Document Server][2] which is a part of ONLYOFFICE Desktop Editors and is used to enable the conversion between the most popular office document formats (DOC, DOCX, ODT, RTF, TXT, PDF, HTML, EPUB, XPS, DjVu, XLS, XLSX, ODS, CSV, PPT, PPTX, ODP).
* [sdkjs](https://github.com/ONLYOFFICE/sdkjs "sdkjs") - JavaScript SDK for the [ONLYOFFICE Document Server][2] which is a part of ONLYOFFICE Desktop Editors and contains API for all the included components client-side interaction.
* [web-apps](https://github.com/ONLYOFFICE/web-apps "web-apps") - the frontend for [ONLYOFFICE Document Server][2] which is a part of ONLYOFFICE Desktop Editors that allows the user to create, edit, save and export text, spreadsheet and presentation documents using the common interface of a document editor.
* [dictionaries](https://github.com/ONLYOFFICE/dictionaries "dictionaries") - the dictionaries of various languages used for spellchecking in ONLYOFFICE Desktop Editors.


## Functionality

ONLYOFFICE Desktop Editors include the following editors:

* ONLYOFFICE Document Editor
* ONLYOFFICE Spreadsheet Editor
* ONLYOFFICE Presentation Editor
 
The editors allow you to create, edit, save and export text, spreadsheet and presentation documents.

## Project Information

Official website: [http://www.onlyoffice.com/apps.aspx](http://www.onlyoffice.com/apps.aspx "http://www.onlyoffice.com/apps.aspx")

Code repository: [https://github.com/ONLYOFFICE/DesktopEditors](https://github.com/ONLYOFFICE/DesktopEditors "https://github.com/ONLYOFFICE/DesktopEditors")

## User Feedback and Support

If you have any problems with or questions about ONLYOFFICE Desktop Editors, please visit our official forum to find answers to your questions: [dev.onlyoffice.org][1] or you can ask and answer ONLYOFFICE development questions on [Stack Overflow][3].

  [1]: http://dev.onlyoffice.org
  [2]: https://github.com/ONLYOFFICE/DocumentServer
  [3]: http://stackoverflow.com/questions/tagged/onlyoffice
