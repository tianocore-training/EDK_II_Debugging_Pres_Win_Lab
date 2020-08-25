---?image=assets/images/gitpitch-audience.jpg
@title[EDK II Debugging]
<br><br><br>
<br><br>
## <span class="gold"   >UEFI & EDK II Training</span>

<span style="font-size:0.95em" >EDK II Debugging w/ Windows Lab</span>
<br>
<span style="font-size:0.75em" >See the <a href="https://github.com/tianocore-training/EDK_II_Debugging_Pres_Win_Lab/blob/master/README.md">Lab Guide</a> to Copy and Paste code examples

<br>
<span style="font-size:0.75em" ><a href='http://www.tianocore.org'>tianocore.org</a></span>
Note:
  PITCHME.md for UEFI / EDK II Training  EDK II Debugging Lab

  Copyright (c) 2020, Intel Corporation. All rights reserved.<BR>

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
 <li>@fa[certificate gp-bullet-gold]<span style="font-size:0.9em">&nbsp;&nbsp;Using PCDs to Configure <font face="Consolas">DebugLib</font> -LAB </span> </li>
 <li>@fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;Change the <font face="Consolas">DebugLib</font> instance to modify the debug<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output - LAB</span> </li>
 <li>@fa[certificate gp-bullet-magenta]<span style="font-size:0.9em">&nbsp;&nbsp;Debug EDK II using VS Debugger - LAB</span> </li>
</ul>


---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 1: Adding Debug Statements]
<br>
<br>
<p align="Left"><span class="gold" ><b>Lab 1: Adding Debug Statements</b></span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab, you'll add debug statements to the previous lab's SampleApp UEFI Shell application</span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
In this lab, you'll learn how to add debug statements.  
This lab uses code from a previous exercise as a starting point (refer to  Writing Simple UEFI Applications).  Before proceeding, verify that the SampleApp code is present in your workspace and that the code references the OvmfPkgX64.dsc file.

---
@title[Lab 1: Catch Up SampleApp]
<p align="right"><span class="gold" ><b>Lab 1: Catch up from previous lab</b></span></p>
@snap[north-west span-60 ]
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" ><br>&nbsp;</span></p>)
@snapend


<span style="font-size:0.8em" >Skip if Lab <a href="https://gitpitch.com/tianocore-training/Writing_UEFI_App_Win_Lab/master#/">Writing UEFI App Lab</a> completed <a href="https://github.com/tianocore-training/Writing_UEFI_App_Win_Lab/blob/master/LabGuide.md">(Lab Guide)</a></span>
<ul style="list-style-type:disc; line-height:0.8;">
   <li><span style="font-size:0.78em" >Perform <a href="https://gitpitch.com/tianocore-training/Platform_Build_Win_Emulator_Lab/master#/">Lab Setup</a> from previous Labs 
   <a href="https://github.com/tianocore-training/Platform_Build_Win_Emulator_Lab/blob/master/Lab_guide.md#slide-10--titlelab-1--build-emulator-section">(Lab Guide)</a> </span></li>
   <li><span style="font-size:0.78em" >Create a Directory under the workspace <font face="Consolas">@size[.8em](C:/FW/edk2-ws/edk2 : "SampleApp")</font></span></li>
   <li><span style="font-size:0.78em" >Copy contents of <font face="Consolas">@size[.8em](C:/../FW/LabSampleCode/SampleAppDebug to C:/FW/edk2-ws/edk2/SampleApp)</font></span></li>
   <li><span style="font-size:0.78em" >Open <font face="Consolas">@size[.8em](C:/FW/edk2-ws/edk2/EmulatorPkg/EmulatorPkg.dsc)</font></span></li>
   <li><span style="font-size:0.78em" >Add the following to the <font face="Consolas">[Components]</font> section: </span></li><br><br><br><br><br>
   <li><span style="font-size:0.78em" >Save and close the file <font face="Consolas">@size[.8em](C:/FW/edk2-ws/edk2/EmulatorPkg/EmulatorPkg.dsc)</font>  </span></li>
</ul>


@snap[north-east span-98 ]
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<p style="line-height:45%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><br><br><br>
&num; Add new modules here<br> &nbsp;
SampleApp/SampleApp.inf
</span></p>
@snapend


Note:

---
@title[Lab 1: Add debug statements SampleApp]
<p align="right"><span class="gold" ><b>Lab 1: Add debug statments to SampleApp</b></span></p>
@snap[north-west span-60 ]
<br>
<br>
<br>
<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br></span></p>

@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" ><br>&nbsp;</span></p>)
<br>
<br>
<br>
<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br></span></p>

@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)

@snapend

<br>
<ul>
  <li><span style="font-size:0.8em" >Open a VS Command Prompt and type <font face="Consolas">@size[.8em](cd C:/FW/edk2-ws/)</font></li><br><br><br>
  <li><span style="font-size:0.78em" >Open <font face="Consolas">@size[.8em](C:/FW/edk2-ws/edk2SampleApp/SampleApp.c)</font> </span></li><br>
  <li><span style="font-size:0.78em" >Add the following to the include statements at the top of the file after below the last "<font face="Consolas">#include</font>" statement: </span></li>
</ul>


@snap[north-east span-98 ]
<br>
<br>
<p style="line-height:45%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><br><br><br><br>
C:/FW/edk2-ws&gt; setenv.bat<br>
C:/FW/edk2-ws&gt; cd edk2 <br>
C:/FW/edk2-ws/edk2&gt; edksetup
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br><br>
&num;include &lt;Library/DebugLib.h&gt;
</span></p>
@snapend

Note:

---
@title[Lab 1: Add debug statements SampleApp 02]
<p align="right"><span class="gold" ><b>Lab 1: Add debug statements to SampleApp</b></span></p>
<p style="line-height:85%"><span style="font-size:0.7em" >Locate the <font face="Consolas">UefiMain</font> function. Then copy and paste the following 
code after the <span style="background-color: #101010">&nbsp;"<font face="Consolas">EFI_INPUT_KEY  KEY;</font>"</span> statement: and before the first <span style="background-color: #101010">&nbsp;<font face="Consolas">Print()</font> </span>statement </span></p>

```c++
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


---?image=/assets/images/slides/Slide7.JPG
@title[Lab 1: Build,Run and Test Result ]
<p align="right"><span class="gold" ><b>Lab 1: Build, Run and Test Result</b></span></p>
@snap[north-west span-50 ]
<br>
<br>
<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)

<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)

<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)
@snapend

@snap[north-west span-50 ]
<br>
<br>
<p style="line-height:80%">
<span style="font-size:0.8em" >At the VS Command Prompt</span></p>
<p style="line-height:45%" align="left" ><span style="font-size:0.57em; font-family:Consolas;" ><br>&nbsp;&nbsp;
  C:/FW/edk2-ws/edk2&gt; Build<br>&nbsp;&nbsp;
  C:/FW/edk2-ws/edk2&gt; RunEmulator
</span></p>
<p style="line-height:80%"  align="left"><span style="font-size:0.8em" >Run the application from the shell</span></p>

<p style="line-height:45%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><br>&nbsp;&nbsp;
<font face="Consolas">@color[yellow](Shell&gt;) &nbsp;SampleApp</font>&nbsp;</span></span></p>

<p style="line-height:80%"  align="left"><span style="font-size:0.8em" >Check the VS Debug output <br>
Exit <br></span></p>
<p style="line-height:45%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><br>&nbsp;&nbsp;
@color[yellow](Shell&gt;) &nbsp;Reset&nbsp;</font></span></span></p>
@snapend

Note:

Notice the changes in DEBUG output for SampleApp in the Visual Studio Command Prompt.  Since the new PCD definitions were only applied to SampleApp, the DEBUG output properties are not changed for other parts of the EmulatorPkg project.
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

---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 2: Changing PCD Value]
<br>
<br>
<p align="Left"><span class="gold" ><b>Lab 2: Changing PCD Value</b></span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab, you'll  learn how to use PCD values to change debugging capabilities. </span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:
In this lab, you'll learn how to use PCD values to change debugging capabilities.  The previous lab,  Adding Debug Statements, did not display all the DEBUG messages added to SampleApp.c.  This lab shows how to change this behavior.




---
@title[Lab 2: Change PCDs for SampleApp]
<p align="right"><span class="gold" ><b>Lab 2: Change PCDs for SampleApp</b></span></p>
@snap[north-west span-100 ]
<br>
<br>
<p style="line-height:20%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" ><br><br><br><br><br>&nbsp;</span></p>)
@snapend

@snap[north-west span-100 ]
<br>
<br>
<p style="line-height:55%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" >
<font face="Arial">@size[1.15em](Open)</font> C:/FW/edk2-ws/edk2EmulatorPkg/EmulatorPkg.dsc <br><br>
<font face="Arial">@size[1.15em](Replace)</font> SampleApp/SampleApp.inf <font face="Arial">@size[1.15em](with the following:)</font><br>
<br><br>&nbsp;&nbsp;
  SampleApp/SampleApp.inf { <br>&nbsp;&nbsp;&nbsp;&nbsp;
    &lt;PcdsFixedAtBuild&gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
      gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0xff<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
      gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0xffffffff<br>&nbsp;&nbsp;
 }<br>
<br><br><br><br>
<font face="Arial">@size[1.15em](Save and close) </font> C:/FW/edk2-ws/edk2EmulatorPkg/EmulatorPkg.dsc
</span></p><br>

@snapend

Note:
- Open C:/FW/edk2-ws/edk2EmulatorPkg/EmulatorPkg.dsc

- Replace SampleApp/SampleApp.inf with the following:


```
SampleApp/SampleApp.inf {
   <PcdsFixedAtBuild>
    gEfiMdePkgTokenSpaceGuid.PcdDebugPropertyMask|0xff
    gEfiMdePkgTokenSpaceGuid.PcdDebugPrintErrorLevel|0xffffffff
 }
```
- save

---?image=/assets/images/slides/Slide10.JPG
@title[Lab 2: Build,Run and Test Result ]
<p align="right"><span class="gold" ><b>Lab 2: Build, Run and Test Result</b></span></p>
@snap[north-west span-50 ]
<br>
<br>
<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)

<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)

<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)
@snapend

@snap[north-west span-50 ]
<br>
<br>
<p style="line-height:80%">
<span style="font-size:0.8em" >At the VS Command Prompt</span></p>
<p style="line-height:45%" align="left" ><span style="font-size:0.57em; font-family:Consolas;" ><br>&nbsp;&nbsp;
  C:/FW/edk2-ws/edk2&gt; Build<br>&nbsp;&nbsp;
  C:/FW/edk2-ws/edk2&gt; RunEmulator
</span></p>
<p style="line-height:80%"  align="left"><span style="font-size:0.8em" >Run the application from the shell</span></p>

<p style="line-height:45%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><br>&nbsp;&nbsp;
<font face="Consolas">@color[yellow](Shell&gt;) &nbsp;SampleApp</font>&nbsp;</span></span></p>

<p style="line-height:80%"  align="left"><span style="font-size:0.8em" >Check the VS Debug output <br>
Exit <br></span></p>
<p style="line-height:45%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><br>&nbsp;&nbsp;
@color[yellow](Shell&gt;) &nbsp;Reset&nbsp;</font></span></span></p>
@snapend



Note:
Notice the changes in DEBUG output for SampleApp in the Visual Studio Command Prompt.  Since the new PCD definitions were only applied to SampleApp, the DEBUG output properties are not changed for other parts of the EmulatorPkg project.

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


---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 3: Library Instances for Debugging]
<br>
<br>
<p align="Left"><span class="gold" ><b>Lab 3: Library Instances for Debugging</b></span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab,  you'll learn how to add specific debug library instances. </span><br>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:

---
@title[Lab 3: Using Library Instances for Debugging]
<p align="right"><span class="gold" ><b>Lab 3: Using Library Instances for Debugging</b></span></p>
@snap[north-west span-100 ]
<br>
<br>
<p style="line-height:20%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" ><br><br><br><br><br>&nbsp;</span></p>)
@snapend

@snap[north-west span-100 ]
<br>
<br>
<p style="line-height:55%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" >
<font face="Arial">@size[1.15em](Open)</font> C:/FW/edk2-ws/edk2/EmulatorPkg/EmulatorPkg.dsc <br><br>
<font face="Arial">@size[1.15em](Replace)</font> SampleApp/SampleApp.inf { . . . } <font face="Arial">@size[1.15em](with the following:)</font><br>
<br><br>&nbsp;&nbsp;
  SampleApp/SampleApp.inf { <br>&nbsp;&nbsp;&nbsp;&nbsp;
    &lt;LibraryClasses &gt;<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
      DebugLib|MdePkg/Library/UefiDebugLibConOut/UefiDebugLibConOut.inf<br>&nbsp;&nbsp;
 }<br>
<br><br><br><br>
<font face="Arial">@size[1.15em](Save and close) </font> C:/FW/edk2-ws/edk2/EmulatorPkg/EmulatorPkg.dsc
</span></p><br>

@snapend

Note:
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


---?image=/assets/images/slides/Slide13.JPG
@title[Lab 3: Build, Run and Test Result]
<p align="right"><span class="gold" ><b>Lab 3: Build, Run and Test Result</b></span></p>
@snap[north-west span-50 ]
<br>
<br>
<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)

<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)
<br>
<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)
@snapend

@snap[north-west span-60 ]
<br>
<br>
<p style="line-height:80%">
<span style="font-size:0.8em" >At the VS Command Prompt</span></p>
<p style="line-height:45%" align="left" ><span style="font-size:0.57em; font-family:Consolas;" ><br>&nbsp;&nbsp;
  C:/FW/edk2-ws/edk2&gt; Build<br>&nbsp;&nbsp;
  C:/FW/edk2-ws/edk2&gt; RunEmulator
</span></p>
<p style="line-height:80%"  align="left"><span style="font-size:0.8em" >Run the application from the shell</span></p>

<p style="line-height:45%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><br>&nbsp;&nbsp;
<font face="Consolas">@color[yellow](Shell&gt;) &nbsp;SampleApp</font>&nbsp;<br></span></span></p>

<p style="line-height:70%"  align="left"><span style="font-size:0.75em" >See that the output from the Debug 
statements now goes to the console  <br><br>
Exit</span></p>
<p style="line-height:45%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" >&nbsp;&nbsp;
@color[yellow](Shell&gt;) &nbsp;Reset&nbsp;</font></span></span></p>
@snapend

Note:

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
Notice the Debug messages output to the console 



---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 4: Serial port Instance of DebugLib]
<br>
<br>
<p align="Left"><span class="gold" ><b>Lab 4: Null Instance of <font face="Consolas">DebugLib</font></b></span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab,  you'll change the <font face="Consolas">DebugLib</font> to the Null instance. </span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:

The DEBUG output for SampleApp is no debug output


---
@title[Lab 4: Using Serial port Library Instances]
<p align="right"><span class="gold" ><b>Lab 4: Using Serial port Library Instances</b></span></p>
<br>
<span style="font-size:0.7em" >Open <font face="Consolas">C:/FW/edk2-ws/edk2/EmulatorPkg/EmulatorPkg.dsc</font> </span><br>
<span style="font-size:0.7em" >Replace <font face="Consolas">SampleApp/SampleApp.inf { . . .}</font> with the following:</span><br>
```c
  SampleApp/SampleApp.inf {
   <LibraryClasses>      
      DebugLib|MdePkg/Library/BaseDebugLibNull/BaseDebugLibNull.inf
 }
```
<span style="font-size:0.7em" >Save and close <font face="Consolas">C:/FW/edk2-ws/edk2/EmulatorPkg/EmulatorPkg.dsc</font> </span><br>

Note:

Lab 4

---?image=/assets/images/slides/Slide16.JPG
@title[Lab 4: Build, Run and Test Result]
<p align="right"><span class="gold" ><b>Lab 4: Build, Run and Test Result</b></span></p>
@snap[north-west span-50 ]
<br>
<br>
<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)

<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)
<br>
<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)
@snapend

@snap[north-west span-60 ]
<br>
<br>
<p style="line-height:80%">
<span style="font-size:0.8em" >At the VS Command Prompt</span></p>
<p style="line-height:45%" align="left" ><span style="font-size:0.57em; font-family:Consolas;" ><br>&nbsp;&nbsp;
  C:/FW/edk2-ws/edk2&gt; Build<br>&nbsp;&nbsp;
  C:/FW/edk2-ws/edk2&gt; RunEmulator
</span></p>
<p style="line-height:80%"  align="left"><span style="font-size:0.8em" >Run the application from the shell</span></p>

<p style="line-height:45%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><br>&nbsp;&nbsp;
<font face="Consolas">@color[yellow](Shell&gt;) &nbsp;SampleApp</font>&nbsp;<br><br></span></span></p>

<p style="line-height:70%"  align="left"><span style="font-size:0.75em" >Check - now @color[red](<b>NO</b>) Debug output <br><br>
Exit</span></p>
<p style="line-height:45%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" >&nbsp;&nbsp;
@color[yellow](Shell&gt;) &nbsp;Reset&nbsp;</font></span></span></p>
@snapend

Note:

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

Notice NO Debug messages output to the console or cmd Prompt




---?image=/assets/images/slides/Slide_LabSec.JPG
@title[Lab 5: Debugging EDK II with VS Debugger]
<br>
<br>
<p align="Left"><span class="gold" ><b>Lab 5: Debugging EDK II with VS Debugger</b></span></p>
<br>
<div class="left1">
<span style="font-size:0.8em" >In this lab,  you'll learn how setup the VS to debug the EDK II emulation</span>
</div>
<div class="right1">
<span style="font-size:0.8em" >&nbsp;  </span>
</div>

Note:


---?image=/assets/images/slides/Slide18.JPG
@title[Lab 5: Emulator Debug with VS]
<p align="right"><span class="gold" ><b>Lab 5: Debug with VS</b></span></p>
<br>
<span style="font-size:0.7em" >Edit the <font face="Consolas">SampleApp.c</font>and add the "<font face="Consolas">ASSERT_EFI_ERROR</font>" Statement :  </span><br>
```c
    EFI_STATUS      Status;
	Status = EFI_NO_RESPONSE;  // or any other EFI Error 
             . . .  
    ASSERT_EFI_ERROR(Status);

```
<br>
<br>
<br>
<br>
<br>

<span style="font-size:0.8em" >Save <font face="Consolas">SampleApp.c</font> </span><br>

Note:
Lab 5, add ASSERT

---?image=/assets/images/slides/Slide19.JPG
@title[Lab 5: Emulator Debug with VS - ASSERT ]
<p align="right"><span class="gold" ><b>Lab 5: Debug with VS - ASSERT  </b></span></p>
@snap[north-west span-50 ]
<br>
<br>
<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)

<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)
<br>
@snapend

@snap[north-west span-60 ]
<br>
<br>
<p style="line-height:80%">
<span style="font-size:0.8em" >At the VS Command Prompt</span></p>
<p style="line-height:45%" align="left" ><span style="font-size:0.57em; font-family:Consolas;" ><br>&nbsp;&nbsp;
  C:/FW/edk2-ws/edk2&gt; Build<br>&nbsp;&nbsp;
  C:/FW/edk2-ws/edk2&gt; RunEmulator
</span></p>
<p style="line-height:80%"  align="left"><span style="font-size:0.8em" >Run the application from the shell</span></p>

<p style="line-height:45%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><br>&nbsp;&nbsp;
<font face="Consolas">@color[yellow](Shell&gt;) &nbsp;SampleApp</font>&nbsp;<br><br></span></span></p>

<p style="line-height:70%"  align="left"><span style="font-size:0.75em" >Assert in VS Command Prompt<br><br>
</span></p>
@snapend

Note:

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

Lab 5, add ASSERT

---?image=/assets/images/slides/Slide20.JPG
@title[Lab 5: Debug with VS ASSERT 02]
<p align="right"><span class="gold" ><b>Lab 5: Debug with VS - ASSERT  </b></span></p>
<p style="line-height:60%"><span style="font-size:0.7em" >
Windows* VS Debugger
Will Pop UP
</span></p>

Note:
Lab 5, add ASSERT


---?image=/assets/images/slides/Slide21.JPG
@title[Lab 5: Debug with VS- CPU bp]
<p align="right"><span class="gold" ><b>Lab 5: Debug with VS - <font face="Consolas">CpuBreakpoint</font></b></span></p>
<br>
<p style="line-height:60%"><span style="font-size:0.7em" >Edit the <font face="Consolas">SampleApp.c</font> and add the "<font face="Consolas">CpuBreakpoint();</font>" Statement and comment out the "<font face="Consolas">ASSERT</font>":  </span></p>
```c
    CpuBreakpoint();
```
<br>
<br>
<br>
<br>
<br>

<span style="font-size:0.8em" >Save <font face="Consolas">SampleApp.c</font> </span><br>

Note:
Lab 5, add CpuBreakpoint();


---?image=/assets/images/slides/Slide22.JPG
@title[Lab 5:  Debug with VS - CPU bp 02]
<p align="right"><span class="gold" ><b>Lab 5: Debug with VS - <font face="Consolas">CpuBreakpoint</font></b></span></p>
@snap[north-west span-50 ]
<br>
<br>
<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)

<p style="line-height:5%" align="left" ><span style="font-size:0.15em; font-family:Consolas;" ><br><br><br></span></p>
@box[bg-black text-white rounded my-box-pad2  ](<p style="line-height:60% "><span style="font-size:0.5em;" >&nbsp;</span></p>)
<br>
@snapend

@snap[north-west span-60 ]
<br>
<br>
<p style="line-height:80%">
<span style="font-size:0.8em" >At the VS Command Prompt</span></p>
<p style="line-height:45%" align="left" ><span style="font-size:0.57em; font-family:Consolas;" ><br>&nbsp;&nbsp;
  C:/FW/edk2-ws/edk2&gt; Build<br>&nbsp;&nbsp;
  C:/FW/edk2-ws/edk2&gt; RunEmulator
</span></p>
<p style="line-height:80%"  align="left"><span style="font-size:0.8em" >Run the application from the shell</span></p>

<p style="line-height:45%" align="left" ><span style="font-size:0.5em; font-family:Consolas;" ><br>&nbsp;&nbsp;
<font face="Consolas">@color[yellow](Shell&gt;) &nbsp;SampleApp</font>&nbsp;<br><br></span></span></p>

<p style="line-height:70%"  align="left"><span style="font-size:0.75em" >VS option to go to VS Debugger<br><br>
</span></p>
@snapend

Note:

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



---?image=/assets/images/slides/Slide23.JPG
@title[Invoke Windows Visual Studio Debugger ]
<p align="right"><span class="gold" ><b>Invoke Windows Visual Studio Debugger</b></span></p>


Note:
- Click yes
- Click Break
- Notice that the VS debugger is inside the CpuBreakpoint function. 
- Continue by stepping and 

---?image=/assets/images/slides/Slide24.JPG
@title[Invoke Windows Visual Studio Debugger 02]
<p align="right"><span class="gold" ><b>Invoke Windows Visual Studio Debugger</b></span></p>

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
  <li>@fa[certificate gp-bullet-gold]<span style="font-size:0.9em">&nbsp;&nbsp;Using PCDs to Configure <font face="Consolas">DebugLib</font> - LAB </span> </li>
 <li>@fa[certificate gp-bullet-yellow]<span style="font-size:0.9em">&nbsp;&nbsp;Change the <font face="Consolas">DebugLib</font> instance to modify the debug<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; output - LAB</span> </li>
 <li>@fa[certificate gp-bullet-magenta]<span style="font-size:0.9em">&nbsp;&nbsp;Debug EDK II using VS Debugger - LAB</span> </li>
</ul>

---?image=assets/images/gitpitch-audience.jpg
@title[Questions]
<br>
![Questions](/assets/images/questions.JPG) 

---
@title[return to main]
<p align="center"><span class="gold"   >@size[1.2em](<b>Return to Main Training Page</b>)</span></p>
<br>
<br>
<br>
<br>
<br>
<p align="center"><span style="font-size:0.9em">Return to Training Table of contents for next presentation <a href="https://github.com/tianocore-training/Tianocore_Training_Contents/wiki#schedule--outline">link</a></span></p>

@snap[north span-30 ]
<br>
<br>
<br>
<a href="https://github.com/tianocore-training/Tianocore_Training_Contents/wiki#schedule--outline">
![trainingLogo](/assets/images/returnTrainingLogo.png)</a>
@snapend

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

Copyright (c) 2020, Intel Corporation. All rights reserved.
**/

```


---?image=assets/images/binary-strings-black2.jpg
@title[Backup Section]
<br><br><br><br><br><br><br>
### <span class="gold"  >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Back up</span>
<span style="font-size:0.9em" >&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>

Note:


---
@title[Issue: Debugging in EmulatorPkg with Windows 7 and VS]
<p align="right"><span class="gold" >Issue:<br>Debugging in EmulatorPkg  with Windows 7 <br>and Visual Studio does not work?</span></p>

<p style="line-height:90%"><span style="font-size:0.9em" >Symptom:  With Windows 7 a CpuBreakpoint() or ASSERT  just exits with an error from the "RunEmulator" command.  </span></p>
<p style="line-height:60%"><span style="font-size:0.7em" >Link to fix this issue: <a href="https://github.com/tianocore/tianocore.github.io/wiki/NT32#Debugging_in_Nt32_Emulation_with_Windows_7_and_Visual_Studio_does_not_work">wiki- Issue Debugging with Windows 7 and Visual Studio </a></span></p>
<ul>
 <li> <span style="font-size:0.7em" >Run the <font face="Consolas">RegEdt32</font> </span> </li>
 <li> <span style="font-size:0.7em" >Navigate to the <font face="Consolas">HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows NT\CurrentVersion\AeDebug</font> </span> </li>
 <li> <span style="font-size:0.7em" >Add a string value entry called <font face="Consolas">"Auto" with a value of "1"</font> </span> </li>
</ul>

<span style="font-size:0.9em" >Windows 10  Visual Studio does not seem to have this issue </span>


