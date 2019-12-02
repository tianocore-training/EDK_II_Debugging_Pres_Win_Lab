## Slide 1  EDK II Debugging]
<br><br><br>

## <span class="gold"   >UEFI & EDK II Training</span>

#### EDK II Debugging w/ Windows Lab

<br>
<span style="font-size:0.75em" ><a href='http://www.tianocore.org'>tianocore.org</a></span>
<!---
Note:

  LabGuid.md for UEFI / EDK II Training  EDK II Debugging Pres-lab

  Copyright (c) 2019, Intel Corporation. All rights reserved.<BR>

  Redistribution and use in source (original document form) and 'compiled'
  forms (converted to PDF, epub, HTML and other formats) with or without
  modification, are permitted provided that the following conditions are met:

  1) Redistributions of source code (original document form) must retain the
     above copyright notice, this list of conditions and the following
     disclaimer as the first lines of this file unmodified.

  2) Redistributions in compiled form (transformed to other DTDs, converted to
     PDF, epub, HTML and other formats) must reproduce the above copyright
     notice, this list of conditions and the following disclaimer in the
     documentation and/or other materials provided with the distribution.

  THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR
  IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
  MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO
  EVENT SHALL TIANOCORE PROJECT  BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
  SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO,
  PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS;
  OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY,
  WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR
  OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF
  ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

<!---  
-->

---

## Slide 2  Lesson Objective
<BR>
<p align="left"<span class="gold"   >Lesson Objective </span></p><br>

<!---  Add bullets using https://fontawesome.com/cheatsheet certificate
-->

- Define DebugLib and its attributes
- List the ways to debug
- Using PCDs to Configure DebugLib - LAB
- Change Compiler & Linker Flags for debugging
- Change the DebugLib instance to modify the debug
        output - LAB
- Debug EDK II using VS Debugger - LAB

---
## Slide 3 Debugging Overview


---
## Slide 4 Debug Methods]
### <p align="left"><span class="gold" ><b>Debug Methods</b></span></p>
<br>
- DEBUG and ASSERT macros in EDK II code 
- DEBUG instead of Print functions 
- Software/hardware debuggers
- Shell commands to test capabilities for simple debugging

Note:
  - Ways to Debug . . .
  - Use DEBUG instead of Print functions in code
  - Use a software debugger (COM/USB)
  - Use a hardware debugger (JTAG/XDB)
  - Soft loading driver through UEFI shell
  - Use shell commands to test capabilities

- What are some alternatives if I don't want to use the debug lib? 

- You can use print statements and soft load my driver to the shell to see what is going on. The downside is that you then have print statements in your code.  This doesn't work really well if you want to make a release to a customer.
- You could use a software debugger, and there is a hardware debugger but if they are not available EDK II debug macro might be a good place to start.

- We believe the debug lib is the simplest and cleanest way to get it all working


---
## Slide 5 EDK II DebugLib Library]
<p align="left"><span style="font-size:01.1em" ><font color="#e49436" ><b>EDK II <font face="Consolas">DebugLib</font> Library</b></font></span></p>

- Debug and Assert macros in code
- Enable/disable when compiled (target.txt)
- Connects a Host to capture debug messages 

Note:
- DebugLib library is clean & very portable
- Using DEBUG and ASSERT macros in code
- Enable/disable when compiled (target.txt)
- Can connect a 2nd PC to capture debug messages 

- The main message-- the debug lib library is portable, it's clean, it's very easy to use, and we believe it's the easiest way to do debugging on a UEFI platform.

- The debug lib library has the debug and assert macros. 
- There are library instances that allow you to use a second PC to capture all messages coming out





---
## Slide 6 Debugging with PCDs]
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Debugging with PCDs </span>

Note:

---
## Slide 7 Using PCDs to Configure DebugLib]
<p align="left"><span class="gold" ><b>Using PCDs to Configure <font face="Consolas">DebugLib</font></b></span></p>



#### MdePkg Debug Library Class
```
[PcdsFixedAtBuild. PcdsPatchableInModule]

  gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0x1f
  gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0x80000040
```

- PCDs Set which drivers report errors and change 
what messages get printed

Note:

- MdePkg Debug Library Class
   - PcdDebugPropertyMask  
- Bit mask to determine which features are on/off
   - PcdDebugPrintErrorLevel 
    - Types of messages produced
	
- PCDs set which drivers report errors and change what messages get printed
- Example from EmulatorPkg.dsc:
  - [PcdsFixedAtBuild.IA32]
    - EfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0x1f
    - gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0x80000040

- How do you configure the debug lib?

- It is configured through PCD entries and some platform configuration database entries.
- This is part of the MDE package, where is the debug library class is defined.
 
- Remember, the library instance can be anywhere, but the library class is defined in the MDE package. This is where the PCDs are defined. 

- It is required for the debug Lib instance to use these PCDs for control.

- PcdDebugPropertyMask and PcdDebugPrintErrorLevel can change sets of driver report errors, and they can also change the error messages that print out.
- Per the example at the bottom of the slide, the assignment for PcdDebugPropertyMask is 0x1f and PcdDebugPrintErrorLevel is 0x80000040 . These are the values for these two PCDs. 


---
## Slide 8 PcdDebugPropertyMask Values
### `PcdDebugPropertyMask`

#### Debug Messages Displayed
<pre>
 #define DEBUG_PROPERTY_DEBUG_ASSERT_ENABLED       0x01
 #define DEBUG_PROPERTY_DEBUG_PRINT_ENABLED        0x02
 #define DEBUG_PROPERTY_DEBUG_CODE_ENABLED         0x04
 #define DEBUG_PROPERTY_CLEAR_MEMORY_ENABLED       0x08
 #define DEBUG_PROPERTY_ASSERT_BREAKPOINT_ENABLED  0x10
 #define DEBUG_PROPERTY_ASSERT_DEADLOOP_ENABLED    0x20
</pre>

- Determines which debugging features are enabled.



Note:
- Enables debugging features
- What kinds of outputs are produced?
- What kind of debugging is being done?

- another Note: Default value in EmulatorPkg is 0x1f

- Determines which debugging features are enabled.

- What kinds of output are produced.
- What kind of debugging is being done.

- Default in Ntr32 is 0x1f

- So what does that mean?
 
- for DebugPropertyMask, 1F turns on:
 	- debug assert, 
	- debug print, 
	- debug code enabled, 
	- clear memory, 
	- assert breakpoint
- It does not turn on assert dead loop.

- This turns on the debug features for the property mask one at a time. The property mask tells the debug command what we really want to have happen.



---
## Slide 9  PcdDebugPrintErrorLevel Values
<p align="left"><span class="gold" ><b>@color[white](<font face="Consolas">PcdDebugPrintErrorLevel</font>) Values</b></span></p>
<p style="line-height:70%" ><span style="font-size:01.0em;" ><font color="#A8ff60"><b>Debugging Messages Displayed</b></font><br></span></p>

```
 #define DEBUG_INIT      0x00000001  // Initialization
 #define DEBUG_WARN      0x00000002  // Warnings
 #define DEBUG_LOAD      0x00000004  // Load events
 #define DEBUG_FS        0x00000008  // EFI File system
 #define DEBUG_POOL      0x00000010  // Alloc & Free's  Pool
 #define DEBUG_PAGE      0x00000020  // Alloc & Free's  Page
 #define DEBUG_INFO      0x00000040  // Verbose
 #define DEBUG_DISPATCH  0x00000080  // PEI/DXE Dispatchers
 #define DEBUG_VARIABLE  0x00000100  // Variable
 #define DEBUG_BM        0x00000400  // Boot Manager
 #define DEBUG_BLKIO     0x00001000  // BlkIo Driver
 #define DEBUG_NET       0x00004000  // SNP / Network Io Driver
 #define DEBUG_UNDI      0x00010000  // UNDI Driver
 #define DEBUG_LOADFILE  0x00020000  // Load File 
 #define DEBUG_EVENT     0x00080000  // Event messages
 #define DEBUG_GCD       0x00100000  // Global Coherency Database changes
 #define DEBUG_CACHE     0x00200000  // Memory range cache-ability changes
 #define DEBUG_VERBOSE   0x00400000  // Detailed debug messages that may
                                     // significantly impact boot performance
 #define DEBUG_ERROR     0x80000000  // Error

    Aliases EFI_D_INIT == DEBUG_INIT, etc...
 
```

Default value in <font face="Consolas">OvmfPkg is 0x8000004f</font>  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Default value in <font face="Consolas">EmulatorPkg is 0x80000040</font>



- Determines which messages we want to print<br>&nbsp;









Note:
- Determines if each print message is displayed
- Use Binary-AND setting to set parameter TRUE
- Note that Aliases EFI_D_INIT == DEBUG_INIT, etc..
- DebugPrintErrorLevel Values

- This has to do with what messages we want to come out

- Let's say you have a debug print enabled as in the previous slide. 
   - You must state what message type you want to print out

- You can assign your own values to this debug print error level.  
- However, these are the guideline values that these drivers use in terms of what their debug output messaging will be.

---
## Slide 10 Changing PCD Values 
<p align="left"><span class="gold" ><b>Changing PCD Values</b></span></p>
- Change all instances of a PCD in platform DSC

```
[PcdsFixedAtBuild.IA32]
gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0x00000000
```

- Change a single module's PCD values in the DSC

```
MyPath/MyModule.inf {
<PcdsFixedAtBuild>
gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0x80000000
}
```

- Minimize message output and minimize size increase<br>&nbsp;</span></p>)

Note:
- Use different PCD values only on the module being debugged
- Minimize message output and minimize size increase



---
## Slide 11 Other Debug Related Libraries  
<p align="left"><span class="gold" ><b>Other Debug Related Libraries </b></span></p>

- ReportStatusCodeLib - Progress codes
	- gEfiMdePkgTokenSpaceGuid.PcdReportStatusCodePropertyMask
- PostCodeLib -  Enable Post codes
	- gEfiMdePkgTokenSpaceGuid.PcdPostCodePropertyMask
- PerformanceLib -  Enable Measurement
	- gEfiMdePkgTokenSpaceGuid.PcdPerformanceLibraryPropertyMask




Note:

- ReportStatusCodeLib - Progress codes
	- gEfiMdePkgTokenSpaceGuid.PcdReportStatusCodePropertyMask
- PostCodeLib -  Enable Post codes
	- gEfiMdePkgTokenSpaceGuid.PcdPostCodePropertyMask
- PerformanceLib -  Enable Measurement
	- gEfiMdePkgTokenSpaceGuid.PcdPerformanceLibraryPropertyMask

---
## Slide 12 Lab 1: Adding Debug Statements

In this lab, you’ll add debug statements to the previous lab's SampleApp UEFI Shell application


Note:
In this lab, you'll learn how to add debug statements.  
This lab uses code from a previous exercise as a starting point (refer to  Writing Simple UEFI Applications).  Before proceeding, verify that the SampleApp code is present in your workspace and that the code references the OvmfPkgX64.dsc file.

---
## Slide 13 Lab 1: Catch Up SampleApp

<p align="left"><span class="gold" ><b>Lab 1: Catch up from previous lab</b></span></p>

Skip if Lab <a href="https://gitpitch.com/tianocore-training/Writing_UEFI_App_Win_Lab/master#/">Writing UEFI App Lab</a> completed<br><BR>

- Perform <a href="https://gitpitch.com/tianocore-training/Platform_Build_Win_Emulator_Lab/master#/">Lab Setup</a> from previous Labs  </span></li>
- Create a Directory under the workspace <font face="Consolas">(C:/FW/edk2-ws/edk2 : "SampleApp")</font></span></li>
- Copy contents of <font face="Consolas">(C:/../FW/LabSampleCode/SampleAppDebug to C:/FW/edk2-ws/edk2/SampleApp)</font></span></li>
- Open <font face="Consolas">(C:/FW/edk2-ws/edk2/EmulatorPkg/EmulatorPkg.dsc)</font></span></li>
- Add the following to the <font face="Consolas">[Components]</font> section: </span></li><br>

```
 # Add new modules here<br> &nbsp;
SampleApp/SampleApp.inf
```
<br>
- Save and close the file <font face="Consolas">(C:/FW/edk2-ws/edk2/EmulatorPkg/EmulatorPkg.dsc)</font>  </span></li>


Note:

---
## Slide 14 Lab 1: Add debug statements SampleApp

Open a VS  Command Prompt and type: cd C:/FW/edk2-ws then
```
    C:/FW/edk2-ws > setenv.bat
    C:/FW/edk2-ws > cd edk2 
    C:/FW/edk2-ws/edk2 > edksetup
```
Open C:/FW/edk2-ws/edk2/SampleApp/SampleApp.c

Add the following to the include statements at the top of the file after below the last “include” statement:
```
	#include <Library/DebugLib.h>
```

Note:

---
## Slide 15 Lab 1: Add debug statements SampleApp 02]
<p align="left"><span class="gold" ><b>Lab 1: Add debug statements to SampleApp</b></span></p>
- Locate the <font face="Consolas">UefiMain</font> function. Then copy and paste the following 
code after the &nbsp;"<font face="Consolas">EFI_INPUT_KEY  KEY;</font>" statement: and before the first &nbsp;<font face="Consolas">Print()</font> statement 


```c
DEBUG ((0xffffffff, "\n\nUEFI Base Training DEBUG DEMO\n") );
DEBUG ((0xffffffff, "0xffffffff USING DEBUG ALL Mask Bits Set\n") );

DEBUG ((DEBUG_INIT,     " 0x%08x USING DEBUG DEBUG_INIT\n" , (UINTN)(DEBUG_INIT))  );
DEBUG ((DEBUG_WARN,     " 0x%08x USING DEBUG DEBUG_WARN\n", (UINTN)(DEBUG_WARN))  );
DEBUG ((DEBUG_LOAD,     " 0x%08x USING DEBUG DEBUG_LOAD\n", (UINTN)(DEBUG_LOAD))  );
DEBUG ((DEBUG_FS,       " 0x%08x USING DEBUG DEBUG_FS\n", (UINTN)(DEBUG_FS))  );
DEBUG ((DEBUG_POOL,     " 0x%08x USING DEBUG DEBUG_POOL\n", (UINTN)(DEBUG_POOL))  );
DEBUG ((DEBUG_PAGE,     " 0x%08x USING DEBUG DEBUG_PAGE\n", (UINTN)(DEBUG_PAGE))  );
DEBUG ((DEBUG_INFO,     " 0x%08x USING DEBUG DEBUG_INFO\n", (UINTN)(DEBUG_INFO))  );
DEBUG ((DEBUG_DISPATCH, " 0x%08x USING DEBUG DEBUG_DISPATCH\n",(UINTN)(DEBUG_DISPATCH)));
DEBUG ((DEBUG_VARIABLE, " 0x%08x USING DEBUG DEBUG_VARIABLE\n",(UINTN)(DEBUG_VARIABLE)));
DEBUG ((DEBUG_BM,       " 0x%08x USING DEBUG DEBUG_BM\n", (UINTN)(DEBUG_BM))  );
DEBUG ((DEBUG_BLKIO,    " 0x%08x USING DEBUG DEBUG_BLKIO\n", (UINTN)(DEBUG_BLKIO))  );
DEBUG ((DEBUG_NET,      " 0x%08x USING DEBUG DEBUG_NET\n", (UINTN)(DEBUG_NET))  );
DEBUG ((DEBUG_UNDI,     " 0x%08x USING DEBUG DEBUG_UNDI\n", (UINTN)(DEBUG_UNDI))  );
DEBUG ((DEBUG_LOADFILE, " 0x%08x USING DEBUG DEBUG_LOADFILE\n",(UINTN)(DEBUG_LOADFILE)));
DEBUG ((DEBUG_EVENT,    " 0x%08x USING DEBUG DEBUG_EVENT\n", (UINTN)(DEBUG_EVENT))  );
DEBUG ((DEBUG_GCD,      " 0x%08x USING DEBUG DEBUG_GCD\n", (UINTN)(DEBUG_EVENT))  );
DEBUG ((DEBUG_CACHE,    " 0x%08x USING DEBUG DEBUG_CACHE\n", (UINTN)(DEBUG_EVENT))  );
DEBUG ((DEBUG_VERBOSE,  " 0x%08x USING DEBUG DEBUG_VERBOSE\n", (UINTN)(DEBUG_EVENT))  );
DEBUG ((DEBUG_ERROR,    " 0x%08x USING DEBUG DEBUG_ERROR\n", (UINTN)(DEBUG_ERROR))  );

```

Note:


---
## Slide 16 Lab 1: Build,Run and Test Result 
<p align="left"><span class="gold" ><b>Lab 1: Build, Run and Test Result</b></span></p>

At the VS Command Prompt
```
   C:/FW/edk2-ws/edk2> Build
   C:/FW/edk2-ws/edk2> RunEmulator
```
Run the application from the shell
```

   Shell>  SampleApp 
```
Check the VS Debug output
Exit

```
   Shell>  Reset 
```

Note:

Notice the changes in DEBUG output for SampleApp in the Visual Studio Command Prompt.  Since the new PCD definitions were only applied to SampleApp, the DEBUG output properties are not changed for other parts of the Emulator project.
At the VS Command Prompt


## Slide 17 Lab 2: Changing PCD Value
<br>
<br>
In this lab, you’ll learn how to use PCD values to change debugging capabilities

Note:
In this lab, you'll learn how to use PCD values to change debugging capabilities.  The previous lab,  Adding Debug Statements, did not display all the DEBUG messages added to SampleApp.c.  This lab shows how to change this behavior.




---
## Slide 18  Lab 2: Change PCDs for SampleApp
<p align="left"><span class="gold" ><b>Lab 2: Change PCDs for SampleApp</b></span></p>

Open  C:/FW/edk2-ws/edk2/EmulatorPkg/EmulatorPkg.dsc
Replace SampleApp/SampleApp.inf with the following:
```
SampleApp/SampleApp.inf {
   <PcdsFixedAtBuild>
    gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0xff
    gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0xffffffff
 }
```
Save and close EmulatorPkg.dsc


Note:

---
## Slide 19 Lab 2: Build,Run and Test Result 
<p align="left"><span class="gold" ><b>Lab 2: Build, Run and Test Result</b></span></p>

Notice the changes in DEBUG output for SampleApp in the Visual Studio Command Prompt.  Since the new PCD definitions were only applied to SampleApp, the DEBUG output properties are not changed for other parts of the  Emulator project.

At the VS Command Prompt
```
C:/FW/edk2-ws/edk2> Build
   C:/FW/edk2-ws/edk2> RunEmulator
```
Run the application from the shell
```

   Shell>  SampleApp 
```
Check the VS Debug output
Exit

```
   Shell>  Reset 
```


## Slide 20 Changing Compiler & Linker Flags Section
<br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Changing Flags</span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Changing Compiler & Linker Flags</span>

Note:

---
## Slide 21 Precedence for Debug Flags Hierarchy
<p align="left"><span class="gold" ><b>Precedence for Debug Flags Hierarchy</b></span></p>

- think of the rules for compiler switches and options as a pyramid
- Pyramid top overrides middle, middle overrides the bottom



- Tools_def.txt
  - Baseline set of command line options for compiler and linker
- INF [BuildOptions] section
  - Append onto existing command line with "="
  - Replace entire existing command line with "=="

- DSC [BuildOptions] section (platform scope)
  - Same usage

- DSC <BuildOptions> under a specific module
  - Same usage 



---
## Slide 22 Compiler / Linker Flags]
<p align="left"><span class="gold" ><b>Compiler / Linker Flags</b></span></p>

<br>
- Example from Microsoft* compiler to turn off optimization
<font face="Consolas">"/02 " to "/01"</font> requires <font face="Consolas">"/0d /01"</font>
<br><br>
Change common flags in platform DSC

```
[BuildOptions]
  DEBUG_*_IA32_CC_FLAGS = /Od /Oy-
```


Change a single module's flags in the DSC
```
MyPath/MyModule.inf {
<BuildOptions>
   DEBUG_*_IA32_CC_FLAGS = /Od /Oy-
}
```



Note:
- Change common flags in platform DSC
 - `[BuildOptions]`
   - DEBUG_*_IA32_CC_FLAGS = /Od

- Change a single module's flags in DSC
 - MyPath/MyModule.inf 
  - `<BuildOptions>`
     - `DEBUG_*_IA32_CC_FLAGS = /Od` 
 

- Change optimizations, etc. . .


---
## Slide 23 DebugLib Usage Section

### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font face="Consolas">DebugLib</font> Usage</span>

Note:


---
## Slide 24 DebugLib Class
<p align="left"><span class="gold" ><b>The <font face="Consolas">DebugLib</font> Class</b></span></p>


- This is the interface so it will be describing the title of the function and / or Macro not the implementation
- The interface will describe the parameters needed 

- MdePkg\Include\Library\DebugLib.h

- Do not call internal worker functions directly
- Macros are where the PCDs are checked
```
   ASSERT (Expression)
   DEBUG (Expression)
   ASSERT_EFI_ERROR (StatusParameter)
   ASSERT_PROTOCOL_ALREADY_INSTALLED(...)
```
- Advanced Macros:
```
   DEBUG_CODE (Expression)
   DEBUG_CODE_BEGIN() & DEBUG_CODE_END()
   DEBUG_CLEAR_MEMORY(...)
```



## Slide 25 DebugLib Instances (1)
<br>
<p align="left"><span class="gold" ><b><font face="Consolas">DebugLib</font> Instances (1)</b></span></p>

#### `BaseDebugLibSerialPort`


- Instance of DebugLib
- Uses SerialPortLib class to send debug output to serial port
- Default for many platforms: BaseDebugLibNull
- OVMF uses it with Switch DEBUG_ON_SERIAL_PORT


Note:
- debugLib library instances

- The first debug Lib library instance is the BaseDebugLibSerialPort 
 this is a debug Lib that is good for PEI and DXE.
 It uses the serial Port Lib class and sends all the debug information out the serial port.
 
- Every time that you type DEBUG, it prints the information to the serial port such that if there is another PC capturing that information out the serial port, it allows easy viewing of the debug information. 
- This is good because it works early on in the platform. You can run very early and get a lot of debug information. 
- At this point the only serial port library instance that is in the public domain or open source is the DUET version 




## Slide 26 DebugLib Instances (2)
<br>
<p align="left"><span class="gold" ><b><font face="Consolas">DebugLib</font> Instances (2)</b></span></p>

#### `UefiDebugLibConOut     or      UefiDebugLibStdErr`<br>

- Instances of DebugLib (for apps and drivers)
- Send all debug output to console/debug console



Note:
- UefiDebugLibConOut   UefiDebugLibStdErr
- Instances of DebugLib (for Apps and Drivers)
- Send all debug output out to console/debug console
- This allows for viewing of debug information
- Make sure that the console is visible



## Slide 27 DebugLib Instances (3)
<br>
<p align="left"><span class="gold" ><b><font face="Consolas">DebugLib</font> Instances (3)</b></span></p>

#### `PeiDxeDebugLibReportStatusCode`<br>

- Sends ASCII String specified by Description Value to the `ReportStatusCode()`
- May also use the SerialPortLib class to send debug output to serial port
- BaseDebugLibNull - Resolves references



- Default for most platforms


Note:
- So there are a total of 5 open source debug lib instances
- The ones we did not cover are "DebugLibNull" - does nothing and 

- "PeiDxeDebugLibReportStatusCode "  is a form of  'DebugLibReportStatusCode"  that wraps into the report status code library the same way that the serial port one does and may send ASCII String  specified by Description Value that is sent to ReportStatusCode() function



- So there may be other instances in your workspace. It is easy to develop a new  library instance. There is no requirement that someone tell us that they've done it. 
- So what you want to do is search for the library name equals in the INF file.
- Example search the INF files in your workspace for the string "LIBRARY_CLASS                  = DebugLib"

- the ASCII string specified by Description is 
  also passed to the handler that displays the POST card value.  Some 
  implementations of this library function may perform I/O operations directly 
  to a POST card device.  Other implementations may send Value to ReportStatusCode(), 



---
## Slide 28 Changing Library Instances ]
<p align="left"><span class="gold" ><b>Changing Library Instances </b></span></p>


Change common library instances in the platform DSC by Module type
```
[LibraryClasses.common.IA32]
DebugLib|MdePkg/Library/BaseDebugLibNull/BaseDebugLibNull.inf
```

Change a single module’s library instance in the platform DSC
```
MyPath/MyModule.inf {
<LibraryClasses>
DebugLib|MdePkg/Library/BaseDebugLibSerialPort.inf
}
```

Note:


Use a different debugging library instance only on the module in question (managing size changes)



---
## Slide 29 Lab 3: Library Instances for Debugging


In this lab, you’ll learn how to add specific debug library instances

Note:

---
## Slide 30 Lab 3: Using Library Instances for Debugging
<p align="left"><span class="gold" ><b>Lab 3: Using Library Instances for Debugging</b></span></p>
- Open C:/FW/edk2-ws/edk2/EmulatorPkg/EmulatorPkg.dsc
- Replace SampleApp/SampleApp.inf { . . .} with the following:

```
SampleApp/SampleApp.inf {
   <LibraryClasses>
    DebugLib|MdePkg/Library/UefiDebugLibConOut/UefiDebugLibConOut.inf
 }
```
- Save and close EmulatorPkgPkg.dsc


Lab 3 Changing Library


---
## Slide 31 Lab 3: Build, Run and Test Result]
<p align="left"><span class="gold" ><b>Lab 3: Build, Run and Test Result</b></span></p>

-At the VS Command Prompt
```
   C:/FW/edk2-ws/edk2> Build
   C:/FW/edk2-ws/edk2> RunEmulator 
```
- Run the application from the shell
```
Shell> SampleApp
```
- See that the output from the Debug 
statements now goes to the console 
- Exit 
```
Shell> Reset
```
Note:

Notice the Debug messages output to the console 




## Slide 32 Lab 4: Serial port Instance of DebugLib

<p align="Left"><span class="gold" ><b>Lab 4: Null Instance of <font face="Consolas">DebugLib</font></b></span></p>
<br>
In this lab,  you'll change the <font face="Consolas">DebugLib</font> to the Null instance. 

Note:

The DEBUG output for SampleApp is no debug output


---
## Slide 33 Lab 4: Using Serial port Library Instances
<p align="left"><span class="gold" ><b>Lab 4: Using Serial port Library Instances</b></span></p>
<br>


- C:/FW/edk2-ws/edk2/EmulatorPkg/EmulatorPkg.dsc
- Replace <font face="Consolas">SampleApp/SampleApp.inf { . . .}</font> with the following: <br>

```c
  SampleApp/SampleApp.inf {
   <LibraryClasses>      
      DebugLib|MdePkg/Library/BaseDebugLibNull/BaseDebugLibNull.inf
 }
```
-Save and close <font face="Consolas">C:/FW/edk2-ws/edk2/EmulatorPkg/EmulatorPkg.dsc</font> 

Note:

Lab 4

---
## Slide 34 Lab 4: Build, Run and Test Result]
<p align="left"><span class="gold" ><b>Lab 4: Build, Run and Test Result</b></span></p>



-At the VS Command Prompt
```
   C:/FW/edk2-ws/edk2> Build
   C:/FW/edk2-ws/edk2> RunEmulator 
```
- Run the application from the shell
```
Shell> SampleApp
```
- Check – now NO Debug output

Exit 
```
  Shell> Reset
```


Note:
Notice NO Debug messages output to the console or cmd Prompt





## Slide 35 Lab 5: Debugging EDK II with VS Debugger]
<br>
<br>
<p align="Left"><span class="gold" ><b>Lab 5: Debugging EDK II with VS Debugger</b></span></p>
<br>

In this lab,  you'll learn how setup the VS to debug the EDK II  emulation

Note:



## Slide 36 Lab 5: Emulator Debug with VS]
<p align="left"><span class="gold" ><b>Lab 5:  Debug with VS</b></span></p>
<br>
- Edit the <font face="Consolas">SampleApp.c</font>and add the "<font face="Consolas">ASSERT_EFI_ERROR</font>" Statement : <br>

```c
    EFI_STATUS      Status;
	Status = EFI_NO_RESPONSE;  // or any other EFI Error 
             . . .  
    ASSERT_EFI_ERROR(Status);

```
<br>

- Save <font face="Consolas">SampleApp.c</font> <br>

Note:
Lab 5, add ASSERT


## Slide 37 Lab 5: Emulator Debug with VS - ASSERT ]
<p align="left"><span class="gold" ><b>Lab 5: Debug with VS - ASSERT  </b></span></p>

-At the VS Command Prompt
```
   C:/FW/edk2-ws/edk2> Build
   C:/FW/edk2-ws/edk2> RunEmulator 
```
- Run the application from the shell
```
Shell> SampleApp
```
- Assert in VS Command Prompt

Exit 
```
  Shell> Reset
```


Note:

Lab 5, add ASSERT


## Slide 38 Lab 5: Debug with VS ASSERT 02
<p align="left"><span class="gold" ><b>Lab 5: Debug with VS - ASSERT  </b></span></p>

- Windows* VS Debugger
Will Pop UP

Note:
Lab 5, add ASSERT



## Slide 39 Lab 5: Debug with VS- CPU bp
<p align="left"><span class="gold" ><b>Lab 5: Debug with VS - <font face="Consolas">CpuBreakpoint</font></b></span></p>
<br>
- Edit the <font face="Consolas">SampleApp.c</font> and add the "<font face="Consolas">CpuBreakpoint();</font>" Statement and comment out the "<font face="Consolas">ASSERT</font>": 

```c
    CpuBreakpoint();
```
<br>

- Save <font face="Consolas">SampleApp.c</font> 

Note:
Lab 5, add CpuBreakpoint();



## Slide 40 Lab 5:  Debug with VS - CPU bp 02
<p align="left"><span class="gold" ><b>Lab 5: Debug with VS - <font face="Consolas">CpuBreakpoint</font></b></span></p>


-At the VS Command Prompt
```
   C:/FW/edk2-ws/edk2> Build
   C:/FW/edk2-ws/edk2> RunEmulator 
```
- Run the application from the shell
```
Shell> SampleApp
```
- VS Debugger pops up


Note:




## Slide 41 Invoke Windows Visual Studio Debugger 
<p align="left"><span class="gold" ><b>Invoke Windows Visual Studio Debugger</b></span></p>


Note:
- Click yes
- Click Break
- Notice that the VS debugger is inside the CpuBreakpoint function. 
- Continue by stepping and 


## Slide 42 Invoke Windows Visual Studio Debugger 02]
<p align="left"><span class="gold" ><b>Invoke Windows Visual Studio Debugger</b></span></p>

Note:
Now the visual studio debugger is debugging the sampleapp function and common debug tasks can be done:
- stepping 
- checking local variables
- checking the call stack.



---  
## Slide  43 Summary
<BR>
### <p align="left"><span class="gold"   >Summary </span></p><br>

- Define DebugLib and its attributes
- List the ways to debug
- Using PCDs to Configure DebugLib - LAB
- Change Compiler & Linker Flags for debugging
- Change the DebugLib instance to modify the debug
        output - LAB
- Debug EDK II using VS Debugger - LAB


---
## Slide 44 Questions
<br>

---
## Slide 45 return to main
<p align="left"><span class="gold"   ><b>Return to Main Training Page</b></span></p>
<br>
<br>
Return to Training Table of contents for next presentation <a href="https://github.com/tianocore-training/Tianocore_Training_Contents/wiki#schedule--outline">link</a>

---
## Slide 46 Logo Slide]
<br><br><br>
![Logo Slide](/assets/images/TianocoreLogo.png)


---
## Slide 47 Acknowledgements]
#### <p align="left"><span class="gold"   >Acknowledgements</span></p>

```c++
/**
Redistribution and use in source (original document form) and 'compiled' forms (converted
to PDF, epub, HTML and other formats) with or without modification, are permitted provided
that the following conditions are met:

Redistributions of source code (original document form) must retain the above copyright 
notice, this list of conditions and the following disclaimer as the first lines of this 
file unmodified.

Redistributions in compiled form (transformed to other DTDs, converted to PDF, epub, HTML
and other formats) must reproduce the above copyright notice, this list of conditions and 
the following disclaimer in the documentation and/or other materials provided with the 
distribution.

THIS DOCUMENTATION IS PROVIDED BY TIANOCORE PROJECT "AS IS" AND ANY EXPRESS OR IMPLIED 
WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND 
FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL TIANOCORE PROJECT BE 
LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES 
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, 
DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, 
WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) 
ARISING IN ANY WAY OUT OF THE USE OF THIS DOCUMENTATION, EVEN IF ADVISED OF THE POSSIBILITY 
OF SUCH DAMAGE.

Copyright (c) 2019, Intel Corporation. All rights reserved.
**/

```


---
## Slide 48 Backup Section
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Backup</span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>

Note:


---
## Slide 49 Issue: Debugging in Emulation with Windows 7 and VS
<p align="left"><span class="gold" >Issue:<br>Debugging in  Emulation with Windows 7 <br>and Visual Studio does not work?</span></p>


- Symptom:  With Windows 7 a CpuBreakpoint() or ASSERT  just exits with an error from the “Build Run” command. 


- Link to fix this issue: 
https://github.com/tianocore/tianocore.github.io/wiki/NT32#Debugging_in_Nt32_Emulation_with_Windows_7_and_Visual_Studio_does_not_work

1. Run the RegEdt32
2. Navigate to the HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows NT\CurrentVersion\AeDebug
3. Add a string value entry called "Auto" with a value of "1“

- Windows 10  Visual Studio does not seem to have this issue 
