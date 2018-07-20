---?image=assets/images/gitpitch-audience.jpg
@title[EDK II Debugging]
<br><br><br><br><br>
## <span class="gold"   >UEFI & EDK II Training</span>

#### EDK II Debugging w/ Windows Lab

<br>
<span style="font-size:0.75em" ><a href='http://www.tianocore.org'>tianocore.org</a></span>
Note:
  PITCHME.md for UEFI / EDK II Training  EDK II Debugging Pres-lab

  Copyright (c) 2018, Intel Corporation. All rights reserved.<BR>

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



---  
@title[Lesson Objective]
<BR>
### <p align="center"<span class="gold"   >Lesson Objective </span></p><br>

<!---  Add bullets using https://fontawesome.com/cheatsheet certificate
-->
<ul style="list-style-type:none">
 <li>@fa[certificate gp-bullet-green]<span style="font-size:0.9em">&nbsp;&nbsp;Define `DebugLib` and its attributes</span> </li>
 <li>@fa[certificate gp-bullet-cyan]<span style="font-size:0.9em">&nbsp;&nbsp;List the ways to debug</span></li>
 <li>@fa[certificate gp-bullet-gold]<span style="font-size:0.9em">&nbsp;&nbsp;Using PCDs to Configure `DebugLib` -LAB </span> </li>
 <li>@fa[certificate gp-bullet-ltgreen]<span style="font-size:0.9em">&nbsp;&nbsp;Change Compiler & Linker Flags for debugging</span></li>
 <li>@fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;Change the `DebugLib` instance to modify the debug<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output - LAB</span> </li>
 <li>@fa[certificate gp-bullet-magenta]<span style="font-size:0.9em">&nbsp;&nbsp;Debug EDK II using VS Debugger - LAB</span> </li>
</ul>

---?image=assets/images/binary-strings-black2.jpg
@title[Debugging Overview]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Debugging Overview </span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>

Note:


---?image=/assets/images/slides/Slide4.JPG
@title[Debug Methods]
### <p align="center"><span class="gold" ><b>Debug Methods</b></span></p>
<br>

<div class="left1">
@ul[no-bullet]
- <span style="font-size:0.9em" ><font color="white">`DEBUG` and `ASSERT` macros in EDK II code </font></span><br><br>
- <span style="font-size:0.9em" ><font color="cyan">`DEBUG` instead of `Print` functions </font></span><br><br>
- <span style="font-size:0.9em" ><font color="white">Software/hardware debuggers </font></span><br><br>
- <span style="font-size:0.9em" ><font color="cyan">Shell commands to test capabilities for simple debugging </font></span>
@ulend
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
- Ways to Debug…
- Use DEBUG instead of Print functions in code
- Use a software debugger (COM/USB)
- Use a hardware debugger (JTAG/XDB)
- Soft loading driver through UEFI shell
- Use shell commands to test capabilities

- What are some alternatives if I don’t want to use the debug lib? 

- You can use print statements and soft load my driver to the shell to see what is going on. The downside is that you then have print statements in your code.  This doesn’t work really well if you want to make a release to a customer.
- You could use a software debugger, and there is a hardware debugger but if they are not available EDK II debug macro might be a good place to start.

- We believe the debug lib is the simplest and cleanest way to get it all working

---
@title[EDK II DebugLib Library]
<p align="center"><span style="font-size:01.1em" ><font color="#e49436" ><b>EDK II `DebugLib` Library</b></font></span></p>
<br>
@ul[no-bullet]
- <span style="font-size:01.0em" >&nbsp;<span style="background-color: #fdb819"><b>&nbsp;&nbsp;`Debug` and `Assert` macros in code &nbsp;&nbsp;</b></span></span></p><br><br>
- <span style="font-size:01.0em" >&nbsp;<span style="background-color: #92d050">&nbsp;&nbsp;<b>Enable/disable when compiled ("target.txt")</b>&nbsp;&nbsp;</span></span><br><br><br>
- <span style="font-size:01.0em" >&nbsp;<span style="background-color: #7030a0">&nbsp;&nbsp;<b>Connects a Host to capture debug messages &nbsp;</b>&nbsp;&nbsp;</span></span>
@ulend

Note:
- DebugLib library is clean & very portable
- Using DEBUG and ASSERT macros in code
- Enable/disable when compiled (target.txt)
- Can connect a 2nd PC to capture debug messages 

- The main message-- the debug lib library is portable, it’s clean, it’s very easy to use, and we believe it’s the easiest way to do debugging on a UEFI platform.

- The debug lib library has the debug and assert macros. 
- There are library instances that allow you to use a second PC to capture all messages coming out



---?image=assets/images/binary-strings-black2.jpg
@title[Debugging with PCDs]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Debugging with PCDs </span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>

Note:


---?image=/assets/images/slides/Slide7.JPG
@title[Using PCDs to Configure DebugLib]
<p align="right"><span class="gold" >Using PCDs to Configure `DebugLib`</span></p>

Note:

- MdePkg Debug Library Class
   - PcdDebugPropertyMask  
- Bit mask to determine which features are on/off
   - PcdDebugPrintErrorLevel 
    - Types of messages produced
	
- PCDs set which drivers report errors and change what messages get printed
- Example from Nt32Pkg.dsc:
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


---?image=/assets/images/slides/Slide8.JPG
@title[PcdDebugPropertyMask Values]
<p align="right"><span class="gold" >`PcdDebugPropertyMask` Values</span></p>

Note:
- Enables debugging features
- What kinds of outputs are produced?
- What kind of debugging is being done?

<pre>
 #define DEBUG_PROPERTY_DEBUG_ASSERT_ENABLED       0x01
 #define DEBUG_PROPERTY_DEBUG_PRINT_ENABLED        0x02
 #define DEBUG_PROPERTY_DEBUG_CODE_ENABLED         0x04
 #define DEBUG_PROPERTY_CLEAR_MEMORY_ENABLED       0x08
 #define DEBUG_PROPERTY_ASSERT_BREAKPOINT_ENABLED  0x10
 #define DEBUG_PROPERTY_ASSERT_DEADLOOP_ENABLED    0x20
</pre>
- another Note: Default value in Nt32 is 0x1f

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




---?image=/assets/images/slides/Slide9.JPG
@title[PcdDebugPrintErrorLevel Values]
<p align="right"><span class="gold" >`PcdDebugPrintErrorLevel` Values</span></p>

Note:
- Determines if each print message is displayed
- Use Binary-AND setting to set parameter TRUE
- Note that Aliases EFI_D_INIT == DEBUG_INIT, etc..
- DebugPrintErrorLevel Values

- This has to do with what messages we want to come out

- Let’s say you have a debug print enabled as in the previous slide. 
   - You must state what message type you want to print out

- You can assign your own values to this debug print error level.  
- However, these are the guideline values that these drivers use in terms of what their debug output messaging will be.



---?image=/assets/images/slides/Slide10.JPG
<!-- .slide: data-transition="none" -->
@title[Changing PCD Values ]
<p align="right"><span class="gold" >Changing PCD Values</span></p>


Note:
- Use different PCD values only on the module being debugged
- Minimize message output and minimize size increase

+++?image=/assets/images/slides/Slide11.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Changing PCD Values 02 ]
<p align="right"><span class="gold" >Changing PCD Values</span></p>


Note:
- Use different PCD values only on the module being debugged
- Minimize message output and minimize size increase



---?image=/assets/images/slides/Slide12.JPG
@title[Other Debug Related Libraries  ]
<p align="right"><span class="gold" >Other Debug Related Libraries </span></p>

Note:

- ReportStatusCodeLib –Progress codes
	- gEfiMdePkgTokenSpaceGuid.PcdReportStatusCodePropertyMask
- PostCodeLib – Enable Post codes
	- gEfiMdePkgTokenSpaceGuid.PcdPostCodePropertyMask
- PerformanceLib – Enable Measurement
	- gEfiMdePkgTokenSpaceGuid.PcdPerformanceLibraryPropertyMask

---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 1: Adding Debug Statements]
<br>
<br>
<p align="Left"><span class="gold" >Lab 1: Adding Debug Statements</span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab, you’ll add debug statements to the previous lab's SampleApp UEFI Shell application</span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
In this lab, you’ll learn how to add debug statements.  
This lab uses code from a previous exercise as a starting point (refer to  Writing Simple UEFI Applications).  Before proceeding, verify that the SampleApp code is present in your workspace and that the code references the OvmfPkgX64.dsc file.

---
@title[Lab 1: Catch Up SampleApp]
<p align="right"><span class="gold" >Lab 1: Catch up from previous lab</span></p>
<span style="font-size:0.8em" >Skip if Lab <a href="https://gitpitch.com/Laurie0131/Writing_UEFI_App_Win_Lab/master#/">Writing UEFI App Lab</a> completed</span>
<ul>
   <li><span style="font-size:0.8em" >Perform <a href="https://gitpitch.com/Laurie0131/Platform_Build_Win_LAB/master#/">Lab Setup</a> from previous Labs  </span></li>
   <li><span style="font-size:0.8em" >Create a Directory under the workspace `C:/FW/edk2  ` : "`SampleApp`"</span></li>
   <li><span style="font-size:0.8em" >Copy contents of `C:/../FW/LabSampleCode/SampleAppDebug` to `C:/FW/edk2/SampleApp`</span></li>
   <li><span style="font-size:0.8em" >Open `C:/FW/edk2/Nt32Pkg/Nt32Pkg.dsc`</span></li>
   <li><span style="font-size:0.8em" >Add the following to the `[Components]` section: </span></li>
<pre lang="php">
```
    # Add new modules here
    SampleApp/SampleApp.inf
```
</pre>
   <li><span style="font-size:0.8em" >Save and close the file `C:/FW/edk2/Nt32Pkg/Nt32Pkg.dsc`  </span></li>
</ul>

Note:

---
@title[Lab 1: Add debug statements SampleApp]
<p align="right"><span class="gold" >Lab 1: Add debug statments to SampleApp</span></p>
<br>
<ul>
  <li><span style="font-size:0.8em" >Open a VS Command Prompt and type `cd C:/FW/edk2` then <br></span>&nbsp;&nbsp;&nbsp;<span style="font-size:0.6em" ><span style="background-color: #101010">&nbsp;` C:/FW/edk2> edksetup `&nbsp;</span> </span></li><br>
  <li><span style="font-size:0.8em" >Open `C:/FW/edk/SampleApp/SampleApp.c` </span></li><br>
  <li><span style="font-size:0.8em" >Add the following to the include statements at the top of the file after below the last “`#include`” statement: </span></li>
<pre>
```
 #include <Library/DebugLib.h>
```
</pre>
</ul>

Note:

---
@title[Lab 1: Add debug statements SampleApp 02]
<p align="right"><span class="gold" >Lab 1: Add debug statements to SampleApp</span></p>
<p style="line-height:90%"><span style="font-size:0.7em" >Locate the `UefiMain` function. Then copy and paste the following code after the <span style="background-color: #101010">&nbsp;“`EFI_INPUT_KEY  KEY;`”</span> statement: and before the first <span style="background-color: #101010">&nbsp;`Print()` </span>statement </span></p>
```c
DEBUG ((0xffffffff, "\n\nUEFI Base Training DEBUG DEMO\n") );
DEBUG ((0xffffffff, "0xffffffff USING DEBUG ALL Mask Bits Set\n") );

DEBUG ((EFI_D_INIT,     " 0x%08x USING DEBUG EFI_D_INIT\n" , (UINTN)(EFI_D_INIT))  );
DEBUG ((EFI_D_WARN,     " 0x%08x USING DEBUG EFI_D_WARN\n", (UINTN)(EFI_D_WARN))  );
DEBUG ((EFI_D_LOAD,     " 0x%08x USING DEBUG EFI_D_LOAD\n", (UINTN)(EFI_D_LOAD))  );
DEBUG ((EFI_D_FS,       " 0x%08x USING DEBUG EFI_D_FS\n", (UINTN)(EFI_D_FS))  );
DEBUG ((EFI_D_POOL,     " 0x%08x USING DEBUG EFI_D_POOL\n", (UINTN)(EFI_D_POOL))  );
DEBUG ((EFI_D_PAGE,     " 0x%08x USING DEBUG EFI_D_PAGE\n", (UINTN)(EFI_D_PAGE))  );
DEBUG ((EFI_D_INFO,     " 0x%08x USING DEBUG EFI_D_INFO\n", (UINTN)(EFI_D_INFO))  );
DEBUG ((EFI_D_DISPATCH, " 0x%08x USING DEBUG EFI_D_DISPATCH\n",(UINTN)(EFI_D_DISPATCH)));
DEBUG ((EFI_D_VARIABLE, " 0x%08x USING DEBUG EFI_D_VARIABLE\n",(UINTN)(EFI_D_VARIABLE)));
DEBUG ((EFI_D_BM,       " 0x%08x USING DEBUG EFI_D_BM\n", (UINTN)(EFI_D_BM))  );
DEBUG ((EFI_D_BLKIO,    " 0x%08x USING DEBUG EFI_D_BLKIO\n", (UINTN)(EFI_D_BLKIO))  );
DEBUG ((EFI_D_NET,      " 0x%08x USING DEBUG EFI_D_NET\n", (UINTN)(EFI_D_NET))  );
DEBUG ((EFI_D_UNDI,     " 0x%08x USING DEBUG EFI_D_UNDI\n", (UINTN)(EFI_D_UNDI))  );
DEBUG ((EFI_D_LOADFILE, " 0x%08x USING DEBUG EFI_D_LOADFILE\n",(UINTN)(EFI_D_LOADFILE)));
DEBUG ((EFI_D_EVENT,    " 0x%08x USING DEBUG EFI_D_EVENT\n", (UINTN)(EFI_D_EVENT))  );
DEBUG ((EFI_D_ERROR,    " 0x%08x USING DEBUG EFI_D_ERROR\n", (UINTN)(EFI_D_ERROR))  );

```

Note:


---?image=/assets/images/slides/Slide17.JPG
@title[Lab 1: Add debug statements SampleApp 03]
<p align="right"><span class="gold" >Lab 1: Add debug statements to SampleApp</span></p>

Note:

---?image=/assets/images/slides/Slide18.JPG
@title[Lab 1: Build,Run and Test Result ]
<p align="right"><span class="gold" >Lab 1: Build, Run and Test Result</span></p>
<br>
<div class="left">
<span style="font-size:0.8em" >At the VS Command Prompt</span>
<pre>
```
    C:/FW/edk2> Build
  C:/FW/edk2> Build Run
```
</pre>
<p style="line-height:90%"><span style="font-size:0.8em" >Run the application from the shell</span><br>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`SampleApp`&nbsp;</span></span></p>
<p style="line-height:90%"><span style="font-size:0.8em" >Check the VS Debug output </span></p>
<p style="line-height:90%"><span style="font-size:0.8em" >Exit <br></span>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`Reset`&nbsp;</span></span></p>
</div>
<div class="right">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:

Notice the changes in DEBUG output for SampleApp in the Visual Studio Command Prompt.  Since the new PCD definitions were only applied to SampleApp, the DEBUG output properties are not changed for other parts of the NT32 project.


---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 2: Changing PCD Value]
<br>
<br>
<p align="Left"><span class="gold" >Lab 2: Changing PCD Value</span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab, you’ll  learn how to use PCD values to change debugging capabilities. </span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
In this lab, you’ll learn how to use PCD values to change debugging capabilities.  The previous lab,  Adding Debug Statements, did not display all the DEBUG messages added to SampleApp.c.  This lab shows how to change this behavior.




---
@title[Lab 2: Change PCDs for SampleApp]
<p align="right"><span class="gold" >Lab 2: Change PCDs for SampleApp</span></p>
<br>
<br>
<span style="font-size:0.7em" >Open `C:/FW/edk/Nt32Pkg/Nt32Pkg.dsc` </span><br>
<span style="font-size:0.7em" >Replace `SampleApp/SampleApp.inf` with the following:</span><br>
```c
  SampleApp/SampleApp.inf {
    <PcdsFixedAtBuild>
      gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0xff
      gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0xffffffff
 }
```
<span style="font-size:0.7em" >Save and close `C:/FW/edk/Nt32Pkg/Nt32Pkg.dsc` </span><br>


Note:


---?image=/assets/images/slides/Slide21.JPG
@title[Lab 2: Build,Run and Test Result ]
<p align="right"><span class="gold" >Lab 2: Build, Run and Test Result</span></p>
<br>
<div class="left">
<span style="font-size:0.8em" >At the VS Command Prompt</span>
<pre>
```
 C:/FW/edk2> Build
 C:/FW/edk2> Build Run
```
</pre>
<p style="line-height:90%"><span style="font-size:0.8em" >Run the application from the shell</span><br>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`SampleApp`&nbsp;</span></span></p>
<p style="line-height:90%"><span style="font-size:0.8em" >Check the VS Debug output </span></p>
<p style="line-height:90%"><span style="font-size:0.8em" >Exit <br></span>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`Reset`&nbsp;</span></span></p>
</div>
<div class="right">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
Notice the changes in DEBUG output for SampleApp in the Visual Studio Command Prompt.  Since the new PCD definitions were only applied to SampleApp, the DEBUG output properties are not changed for other parts of the NT32 project.


---?image=assets/images/binary-strings-black2.jpg
@title[Changing Compiler & Linker Flags Section]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Changing Flags</span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Changing Compiler & Linker Flags</span>

Note:

---?image=/assets/images/slides/Slide23.JPG
<!-- .slide: data-transition="none" -->
@title[Precedence for Debug Flags Hierarchy]
<p align="right"><span class="gold" >Precedence for Debug Flags Hierarchy</span></p>

Note:


+++?image=/assets/images/slides/Slide24.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Precedence for Debug Flags Hierarchy 02]
<p align="right"><span class="gold" >Precedence for Debug Flags Hierarchy</span></p>

Note:

- think of the rules for compiler switches and options as a pyramid
 - Pyramid top overrides middle, middle overrides the bottom



+++?image=/assets/images/slides/Slide25.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Precedence for Debug Flags Hierarchy 03]
<p align="right"><span class="gold" >Precedence for Debug Flags Hierarchy</span></p>

Note:


+++?image=/assets/images/slides/Slide26.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Precedence for Debug Flags Hierarchy 04]
<p align="right"><span class="gold" >Precedence for Debug Flags Hierarchy</span></p>

Note:


+++?image=/assets/images/slides/Slide27.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Precedence for Debug Flags Hierarchy 05]
<p align="right"><span class="gold" >Precedence for Debug Flags Hierarchy</span></p>

Note:


---?image=/assets/images/slides/Slide28.JPG
<!-- .slide: data-transition="none" -->
@title[Compiler / Linker Flags]
<p align="right"><span class="gold" >Compiler / Linker Flags</span></p>

Note:
- Change common flags in platform DSC
 - [BuildOptions]
   - DEBUG_*_IA32_CC_FLAGS = /Od

- Change a single module’s flags in DSC
 - MyPath/MyModule.inf {
  - <BuildOptions>
     - DEBUG_*_IA32_CC_FLAGS = /Od 
 - }

- Change optimizations, etc…


+++?image=/assets/images/slides/Slide29.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Compiler / Linker Flags 02]
<p align="right"><span class="gold" >Compiler / Linker Flags</span></p>

Note:
- Change common flags in platform DSC
 - [BuildOptions]
   - DEBUG_*_IA32_CC_FLAGS = /Od

- Change a single module’s flags in DSC
 - MyPath/MyModule.inf {
  - <BuildOptions>
     - DEBUG_*_IA32_CC_FLAGS = /Od 
 - }

- Change optimizations, etc…


+++?image=/assets/images/slides/Slide30.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Compiler / Linker Flags 03]
<p align="right"><span class="gold" >Compiler / Linker Flags</span></p>

Note:
- Change common flags in platform DSC
 - [BuildOptions]
   - DEBUG_*_IA32_CC_FLAGS = /Od

- Change a single module’s flags in DSC
 - MyPath/MyModule.inf {
  - <BuildOptions>
     - DEBUG_*_IA32_CC_FLAGS = /Od 
 - }

- Change optimizations, etc…


---?image=assets/images/binary-strings-black2.jpg
@title[DebugLib Usage Section]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`DebugLib` Usage</span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>

Note:


---?image=/assets/images/slides/Slide32.JPG
@title[DebugLib Class]
<p align="center"><span class="gold" ><b>The `DebugLib` Class</b></span></p>

Note:

- This is the interface so it will be describing the title of the function and / or Macro not the implementation
- The interface will describe the parameters needed 

- MdePkg\Include\Library\DebugLib.h

- Do not call internal worker functions directly
- Macros are where the PCDs are checked
  - ASSERT (Expression)
  - DEBUG (Expression)
  - ASSERT_EFI_ERROR (StatusParameter)
  - ASSERT_PROTOCOL_ALREADY_INSTALLED(…)

  - Advanced Macros:
  - DEBUG_CODE (Expression)
  - DEBUG_CODE_BEGIN() & DEBUG_CODE_END()
  - DEBUG_CLEAR_MEMORY(…)



---?image=/assets/images/slides/Slide33.JPG
@title[DebugLib Instances (1)]
<br>
<p align="left"><span class="gold" ><b>`DebugLib` Instances (1)</b></span></p>
<br>
<br>
<ul>
  <li><span style="font-size:0.9em">Instance of `DebugLib`   </span></li>
  <li><span style="font-size:0.9em">Uses `SerialPortLib` class to send debug output to serial port</span></li>
  <li><span style="font-size:0.9em">Default for many platforms:  `BaseDebugLibNull`  </span></li>
  <li><span style="font-size:0.9em">OVMF uses it with Switch `DEBUG_ON_SERIAL_PORT`  </span></li>
</ul>


Note:
- debugLib library instances

- The first debug Lib library instance is the BaseDebugLibSerialPort 
 this is a debug Lib that is good for PEI and DXE.
 It uses the serial Port Lib class and sends all the debug information out the serial port.
 
- Every time that you type DEBUG, it prints the information to the serial port such that if there is another PC capturing that information out the serial port, it allows easy viewing of the debug information. 
- This is good because it works early on in the platform. You can run very early and get a lot of debug information. 
- At this point the only serial port library instance that is in the public domain or open source is the DUET version 


---?image=/assets/images/slides/Slide34.JPG
@title[DebugLib Instances (2)]
<br>
<p align="left"><span class="gold" ><b>`DebugLib` Instances (2)</b></span></p>
<br>
<br>
<br>
<ul>
  <li><span style="font-size:0.9em">Instances of `DebugLib` (for apps and drivers)</span></li><br>
  <li><span style="font-size:0.9em">Send all debug output to console/debug console</span></li>
</ul>


Note:
- UefiDebugLibConOut   UefiDebugLibStdErr
- Instances of DebugLib (for Apps and Drivers)
- Send all debug output out to console/debug console
- This allows for viewing of debug information
- Make sure that the console is visible


---?image=/assets/images/slides/Slide35.JPG
@title[DebugLib Instances (3)]
<br>
<p align="left"><span class="gold" ><b>`DebugLib` Instances (3)</b></span></p>
<br>
<br>
<ul>
  <li><span style="font-size:0.9em">Sends ASCII String  specified by  Description Value to the `ReportStatusCode()`  </span></li>
  <li><span style="font-size:0.9em">May also use the `SerialPortLib` class to send debug output to serial port</span></li>
  <li><span style="font-size:0.9em">`BaseDebugLibNull`  - Resolves references </span></li>
</ul>
<br>
<p align="center"><span style="font-size:0.9em"><font color="blue">Default for most platforms</font></span></p>



Note:
- So there are a total of 5 open source debug lib instances
- The ones we did not cover are “DebugLibNull” – does nothing and 

- “PeiDxeDebugLibReportStatusCode “  is a form of  “DebugLibReportStatusCode”  that wraps into the report status code library the same way that the serial port one does and may send ASCII String  specified by Description Value that is sent to ReportStatusCode() function



- So there may be other instances in your workspace. It is easy to develop a new  library instance. There is no requirement that someone tell us that they’ve done it. 
- So what you want to do is search for the library name equals in the INF file.
- Example search the INF files inyour workspace for the string “LIBRARY_CLASS                  = DebugLib” 

- the ASCII string specified by Description is 
  also passed to the handler that displays the POST card value.  Some 
  implementations of this library function may perform I/O operations directly 
  to a POST card device.  Other implementations may send Value to ReportStatusCode(), 


---?image=/assets/images/slides/Slide36.JPG
@title[Changing Library Instances ]
<p align="right"><span class="gold" >Changing Library Instances </span></p>

Note:
- Change common library instances in the platform DSC by module type
<pre>
  [LibraryClasses.common.IA32]
    DebugLib|MdePkg/Library/BaseDebugLibNull/BaseDebugLibNull.inf
</pre>
- Change a single module’s library instance in the platform DSC
<pre>
  MyPath/MyModule.inf {
    <LibraryClasses>
     DebugLib|MdePkg/Library/BaseDebugLibSerialPort.inf
  }
</pre>
- another Note: Use a different debugging library instance only on the module in question (managing size changes)




+++?image=/assets/images/slides/Slide37.JPG
<!-- .slide: data-background-transition="none" -->
<!-- .slide: data-transition="none" -->
@title[Changing Library Instances 02 ]
<p align="right"><span class="gold" >Changing Library Instances </span></p>



Note:
- Change common library instances in the platform DSC by module type
<pre>
  [LibraryClasses.common.IA32]
    DebugLib|MdePkg/Library/BaseDebugLibNull/BaseDebugLibNull.inf
</pre>
- Change a single module’s library instance in the platform DSC
<pre>
  MyPath/MyModule.inf {
    <LibraryClasses>
     DebugLib|MdePkg/Library/BaseDebugLibSerialPort.inf
  }
</pre>
- another Note: Use a different debugging library instance only on the module in question (managing size changes)

---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 3: Library Instances for Debugging]
<br>
<br>
<p align="Left"><span class="gold" >Lab 3: Library Instances for Debugging</span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab,  you’ll learn how to add specific debug library instances. </span><br>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:


---
@title[Lab 3: Using Library Instances for Debugging]
<p align="right"><span class="gold" >Lab 3: Using Library Instances for Debugging</span></p>
<br>
<br>
<span style="font-size:0.7em" >Open `C:/FW/edk2/Nt32Pkg/Nt32Pkg.dsc` </span><br>
<span style="font-size:0.7em" >Replace `SampleApp/SampleApp.inf { . . .}` with the following:</span><br>
```c
  SampleApp/SampleApp.inf {
   <LibraryClasses>
    DebugLib|MdePkg/Library/UefiDebugLibConOut/UefiDebugLibConOut.inf
 }
```
<span style="font-size:0.7em" >Save and close `C:/FW/edk2/Nt32Pkg/Nt32Pkg.dsc` </span><br>

Note:

Lab 3 Changing Library


---?image=/assets/images/slides/Slide40.JPG
@title[Lab 3: Build, Run and Test Result]
<p align="right"><span class="gold" >Lab 3: Build, Run and Test Result</span></p>
<div class="left">
<span style="font-size:0.8em" >At the VS Command Prompt</span>
<pre>
```
 C:/FW/edk2> Build
 C:/FW/edk2> Build Run
```
</pre>
<p style="line-height:90%"><span style="font-size:0.8em" >Run the application from the shell</span><br>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`SampleApp`&nbsp;</span></span></p>
<p style="line-height:90%"><span style="font-size:0.8em" >See that the output from the Debug statements now goes to the Nt32 console </span></p>
<p style="line-height:90%"><span style="font-size:0.8em" >Exit <br></span>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`Reset`&nbsp;</span></span></p>
</div>
<div class="right">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
Notice the Debug messages output to the console 


---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 4: Serial port Instance of DebugLib]
<br>
<br>
<p align="Left"><span class="gold" >Lab 4: Null Instance of `DebugLib`</span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab,  you’ll change the `DebugLib` to the Null instance. </span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:

The DEBUG output for SampleApp is no debug output


---
@title[Lab 4: Using Serial port Library Instances]
<p align="right"><span class="gold" >Lab 4: Using Serial port Library Instances</span></p>
<br>
<span style="font-size:0.7em" >Open `C:/FW/edk2/Nt32Pkg/Nt32Pkg.dsc` </span><br>
<span style="font-size:0.7em" >Replace `SampleApp/SampleApp.inf { . . .}` with the following:</span><br>
```c
  SampleApp/SampleApp.inf {
   <LibraryClasses>      
      DebugLib|MdePkg/Library/BaseDebugLibNull/BaseDebugLibNull.inf
 }
```
<span style="font-size:0.7em" >Save and close `C:/FW/edk2/Nt32Pkg/Nt32Pkg.dsc` </span><br>

Note:

Lab 4

---?image=/assets/images/slides/Slide43.JPG
@title[Lab 4: Build, Run and Test Result]
<p align="right"><span class="gold" >Lab 4: Build, Run and Test Result</span></p>
<br>
<div class="left">
<span style="font-size:0.8em" >At the VS Command Prompt</span>
<pre>
```
 C:/FW/edk2> Build
 C:/FW/edk2> Build Run
```
</pre>
<p style="line-height:90%"><span style="font-size:0.8em" >Run the application from the shell</span><br>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`SampleApp`&nbsp;</span></span></p>
<p style="line-height:90%"><span style="font-size:0.8em" >Check - now<font color="red"> <b>NO</b> </font>Debug output</span></p>
<p style="line-height:90%"><span style="font-size:0.8em" >Exit <br></span>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`Reset`&nbsp;</span></span></p>
</div>
<div class="right">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
Notice the Debug messages output to the console 




---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 5: Debugging EDK II with VS Debugger]
<br>
<br>
<p align="Left"><span class="gold" >Lab 5: Debugging EDK II with VS Debugger</span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab,  you’ll learn how setup the VS to debug the EDK II Nt32 emulation</span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:


---?image=/assets/images/slides/Slide45.JPG
@title[Lab 5: Nt32 Debug with VS]
<p align="right"><span class="gold" >Lab 5: Nt32 Debug with VS</span></p>
<br>
<span style="font-size:0.7em" >Edit the `SampleApp.c`and add the “`ASSERT_EFI_ERROR`” Statement :  </span><br>
```c
   ASSERT_EFI_ERROR(0x80000000);
```
<br>
<br>
<br>
<br>
<br>

<span style="font-size:0.8em" >Save `SampleApp.c` </span><br>

Note:
Lab 5, add ASSERT

---?image=/assets/images/slides/Slide46.JPG
@title[Lab 5: Nt32 Debug with VS ]
<p align="right"><span class="gold" >Lab 5: Nt32 Debug with VS </span></p>
<br>
<div class="left">
<span style="font-size:0.8em" >At the VS Command Prompt</span>
<pre>
```
 C:/FW/edk2> Build
 C:/FW/edk2> Build Run
```
</pre>
<p style="line-height:90%"><span style="font-size:0.8em" >Run the application from the shell</span><br>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`SampleApp`&nbsp;</span></span></p>
<p style="line-height:90%"><span style="font-size:0.8em" >Assert in VS Command Prompt</font>Debug output</span></p>
</div>
<div class="right">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
Lab 5, add ASSERT

---?image=/assets/images/slides/Slide47.JPG
@title[Lab 5: Nt32 Debug with VS 02]
<p align="right"><span class="gold" >Lab 5: Nt32 Debug with VS </span></p>


Note:
Lab 5, add ASSERT


---?image=/assets/images/slides/Slide48.JPG
@title[Lab 5: Nt32 Debug with VS- CPU bp]
<p align="right"><span class="gold" >Lab 5: Nt32 Debug with VS</span></p>
<br>
<p style="line-height:60%"><span style="font-size:0.7em" >Edit the `SampleApp.c` and add the “`CpuBreakpoint();`” Statement and comment out the “`ASSERT`”:  </span></p>
```c
    CpuBreakpoint();
```
<br>
<br>
<br>
<br>
<br>

<span style="font-size:0.8em" >Save `SampleApp.c` </span><br>

Note:
Lab 5, add CpuBreakpoint();


---?image=/assets/images/slides/Slide49.JPG
@title[Lab 5: Nt32 Debug with VS ]
<p align="right"><span class="gold" >Lab 5: Nt32 Debug with VS </span></p>
<br>
<div class="left">
<span style="font-size:0.8em" >At the VS Command Prompt</span>
<pre>
```
 C:/FW/edk2> Build
 C:/FW/edk2> Build Run
```
</pre>
<p style="line-height:90%"><span style="font-size:0.8em" >Run the application from the shell</span><br>
<span style="font-size:0.5em" ><span style="background-color: #101010">&nbsp;<font color="yellow">`Shell> `&nbsp;</font>`SampleApp`&nbsp;</span></span></p>
<p style="line-height:90%"><span style="font-size:0.8em" >VS option to Debug</font>Debug output</span></p>
</div>
<div class="right">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
Lab 5  add CpuBreakpoint();


---?image=/assets/images/slides/Slide50.JPG
@title[Invoke Windows Visual Studio Debugger ]
<p align="right"><span class="gold" >Invoke Windows Visual Studio Debugger</span></p>


Note:
- Click yes
- Click Break
- Notice that the VS debugger is inside the CpuBreakpoint function. 
- Continue by stepping and 

---?image=/assets/images/slides/Slide51.JPG
@title[Invoke Windows Visual Studio Debugger ]
<p align="right"><span class="gold" >Invoke Windows Visual Studio Debugger</span></p>

Note:
Now the visual studio debugger is debugging the sampleapp function and common debug tasks can be done:
- stepping 
- checking local variables
- checking the call stack.



---  
@title[Summary]
<BR>
### <p align="center"><span class="gold"   >Summary </span></p><br>
<ul style="list-style-type:none">
 <li>@fa[certificate gp-bullet-green]<span style="font-size:0.9em">&nbsp;&nbsp;Define `DebugLib` and its attributes</span> </li>
 <li>@fa[certificate gp-bullet-cyan]<span style="font-size:0.9em">&nbsp;&nbsp;List the ways to debug</span></li>
 <li>@fa[certificate gp-bullet-gold]<span style="font-size:0.9em">&nbsp;&nbsp;Using PCDs to Configure `DebugLib` - LAB </span> </li>
 <li>@fa[certificate gp-bullet-ltgreen]<span style="font-size:0.9em">&nbsp;&nbsp;Change Compiler & Linker Flags for debugging</span></li>
 <li>@fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;Change the `DebugLib` instance to modify the debug<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output - LAB</span> </li>
 <li>@fa[certificate gp-bullet-magenta]<span style="font-size:0.9em">&nbsp;&nbsp;Debug EDK II using VS Debugger - LAB</span> </li>
</ul>

---?image=assets/images/gitpitch-audience.jpg
@title[Questions]
<br>
![Questions](/assets/images/questions.JPG) 


---?image=assets/images/gitpitch-audience.jpg
@title[Logo Slide]
<br><br><br>
![Logo Slide](/assets/images/TianocoreLogo.png =10x)


---
@title[Acknowledgements]
#### <p align="center"><span class="gold"   >Acknowledgements</span></p>

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

Copyright (c) 2018, Intel Corporation. All rights reserved.
**/

```


---?image=assets/images/binary-strings-black2.jpg
@title[Backup Section]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Back up</span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>

Note:


---
@title[Issue: Debugging in Nt32 Emulation with Windows 7 and VS]
<p align="right"><span class="gold" >Issue:<br>Debugging in Nt32 Emulation with Windows 7 <br>and Visual Studio does not work?</span></p>

<p style="line-height:90%"><span style="font-size:0.9em" >Symptom:  With Windows 7 a CpuBreakpoint() or ASSERT  just exits with an error from the “Build Run” command.  </span></p>
<p style="line-height:60%"><span style="font-size:0.7em" >Link to fix this issue: <a href="https://github.com/tianocore/tianocore.github.io/wiki/NT32#Debugging_in_Nt32_Emulation_with_Windows_7_and_Visual_Studio_does_not_work">wiki- Issue Debugging Nt32 with Windows 7 and Visual Studio </a></span></p>
<ul>
 <li> <span style="font-size:0.7em" >Run the `RegEdt32` </span> </li>
 <li> <span style="font-size:0.7em" >Navigate to the `HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows NT\CurrentVersion\AeDebug` </span> </li>
 <li> <span style="font-size:0.7em" >Add a string value entry called "`Auto`" with a value of "`1`“ </span> </li>
</ul>

<span style="font-size:0.9em" >Windows 10  Visual Studio does not seem to have this issue </span>


