# OpenXLSX
OpenXLSX is a C++ library for reading, writing, creating and modifying Microsoft Excel® files, with the .xlsx format.

## Motivation
Many programming languages have the ability to modify Excel files, either natively or in the form of open source libraries. This includes Python, Java and C#. For C++, however, things are more scattered. While there are some libraries, they are generally less mature and have a smaller feature set than for other languages. 

Because there are no open source library that fully fitted my needs, I decided to develop the OpenXLSX library.


## Ambition
The ambition is that OpenXLSX should be able to read, write, create and modify Excel files (data as well as formatting), and do so with as few dependencies as possible. Currently, OpenXLSX depends on the following 3rd party libraries (all included in the repository):

 - PugiXML
 - Zippy (C++ wrapper around miniz)
 
 ## Current Status
 OpenXLSX is still work i progress. The following is a list of features which have been implemented and should be working properly:
 
  - Create/open/save files
  - Read/write/modify cell contents
  - Copy cells and cell ranges
  - Copy worksheets
  
  Features related to formatting, plots and figures have not been implemented, and are not planned to be in the near future.
  
  It should be noted, that creating const XLDocument objects, is currently not working!
  
  ## Compatibility
  OpenXLSX can be built and run on the following platforms/compilers:
  
  - Linux (GCC/Clang)
  - MacOS (GCC/Clang/Xcode)
  - Windows (MinGW/Clang/MSVC)
  
  Building on Windows using MSYS or Cygwin has not been tested.
  Building using the Intel compiler has not been tested.
  
  ## Unicode
  All string manipulations and usage in OpenXLSX uses the C++ std::string, which uses UTF-8 encoding. Also, Excel uses UTF-8 encoding internally (actually, it might be possible to use other encodings, but I'm not sure about that). 
  
  For the above reason, if you work with other text encodings, you have to convert to/from UTF-8 yourself. There are a number of options (e.g. std::codecvt or Boost.Locale). I will also suggest that you see James McNellis' presentation at CppCon 2014: https://youtu.be/n0GK-9f4dl8
  
  ## Usage
  
  ```cpp
  #include <iostream>
  #include <iomanip>
  #include <OpenXLSX/OpenXLSX.h>
  
  using namespace std;
  using namespace OpenXLSX;
  
  int main()
  {
      
      XLDocument doc;
      doc.CreateDocument("./MyTest.xlsx");
      auto wks = doc.Workbook().Worksheet("Sheet1");
  
      wks.Cell("A1").Value() = 3.14159;
      wks.Cell("B1").Value() = 42;
      wks.Cell("C1").Value() = "Hello OpenXLSX!";
      wks.Cell("D1").Value() = true;
      wks.Cell("E1").Value() = wks.Cell("C1").Value();
  
      auto A1 = wks.Cell("A1").Value().Get<double>();
      auto B1 = wks.Cell("B1").Value().Get<unsigned int>();
      auto C1 = wks.Cell("C1").Value().Get<std::string>();
      auto D1 = wks.Cell("D1").Value().Get<bool>();
      auto E1 = wks.Cell("E1").Value().Get<std::string>();
      
      auto val = wks.Cell("E1").Value();
      cout << val.Get<std::string>() << endl;
  
      cout << "Cell A1: " << A1 << endl;
      cout << "Cell B1: " << B1 << endl;
      cout << "Cell C1: " << C1 << endl;
      cout << "Cell D1: " << D1 << endl;
      cout << "Cell E1: " << E1 << endl;
  
      doc.SaveDocument();
  
      return 0;
  }

  ```  
