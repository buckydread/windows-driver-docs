---
title: Installing Just the Checked Operating System and HAL (For Windows Vista and Later)
description: Installing Just the Checked Operating System and HAL (For Windows Vista and Later)
ms.assetid: 1203b7cd-50b9-4174-8bec-112019444fac
---

# Installing Just the Checked Operating System and HAL (For Windows Vista and Later)


Instead of installing the complete checked build on your computer, you can install the free build of the system, and then install the checked versions of the operating system image and the hardware abstraction layer (HAL). If you use this procedure, you can configure the boot loader to provide you with two boot options. One boot option is for the free build. The second boot option starts the system using the checked operating system image and HAL, but uses free versions of all other system components.

-   [Step 1: Identifying the Files To Install for Windows Vista and later](#step-1--identifying-the-files-to-install-for-windows-vistaand-later)
-   [Step 2: Copying the Checked Files](#step-2--copying-the-checked-files)
-   [Step 3: Changing the boot parameters using BCDEdit](#step-3--changing-the-boot-parameters-using-bcdedit)
-   [Step 4: Restart the computer](#step-4--restart-the-computer)

### <span id="step_1__identifying_the_files_to_install_for_windows_vistaand_later"></span><span id="STEP_1__IDENTIFYING_THE_FILES_TO_INSTALL_FOR_WINDOWS_VISTAAND_LATER"></span>Step 1: Identifying the Files To Install for Windows Vista and later

Before you install a partial checked build, you must determine the version of the operating system image and HAL files that were used to install the free build on your system.

**Tip**   For 64-bit versions of Windows Vista and later, this process is easy. If you have the [Windows Driver Kit (WDK)](https://msdn.microsoft.com/library/windows/hardware/ff557573), you can use the operating system image and HAL files from the Debug\\ directory of the WDK. See [Downloading a Checked Build of Windows](obtaining-the-checked-build.md). There is only one version of each for amd64 or ia64. The names of the files are ntkrnlmp.exe and Hal.dll. If you have the WDK for the version of Windows you are using, you can skip ahead to [Step 2: Copying the Checked Files](#step-2--copying-the-checked-files).

 

On computers running the 32-bit version of Windows, use the following procedures to identify the names of the files to copy:

### <span id="determining_the_name_of_the_hal_that_is_installed"></span><span id="DETERMINING_THE_NAME_OF_THE_HAL_THAT_IS_INSTALLED"></span>Determining the name of the HAL that is installed

1.  Open the file %SystemRoot%\\Inf\\setupapi.dev.log and search for hal.dll.

    You should find a line like TargetFilename - 'hal.dll'

2.  In the same section of the log file, look for the corresponding *SourceFilename*. The name to the right of *SourceFilename* is the name of the HAL file that you need to copy from the checked build.

The following example is from a setupapi.dev.log file. The *SourceFilename* is halmacpi.dll:

```
{FILE_QUEUE_COPY}
   CopyStyle      - 0x09180000
   SourceRootPath - &#39;C:\Windows\System32\DriverStore\FileRepository\hal.inf_0c52392f&#39;
   SourceFilename - &#39;halmacpi.dll&#39;
   TargetDirectory- &#39;C:\Windows\system32&#39;
   TargetFilename - &#39;hal.dll&#39;
   SourceDesc     - &#39;windows cd&#39;
{FILE_QUEUE_COPY exit(0x00000000)}
```

### <span id="determining_the_name_of_the_operating_system_image_file_installed"></span><span id="DETERMINING_THE_NAME_OF_THE_OPERATING_SYSTEM_IMAGE_FILE_INSTALLED"></span>Determining the name of the operating system image file installed

For 64-bit versions of Windows Vista and later, the name of the file is ntkrnlmp.exe.

For 32-bit versions Windows Vista, you can use the following procedure to determine whether you have the uniprocessor or multiprocessor version installed.

1.  Open the Event Viewer in the Computer Management Console (compmgmt.msc).

2.  Find the Event ID 6009 in the System Log.

    The properties for this event indicate whether you have a single processor or a multiprocessor version of the operating system image installed.

For example, the following indicates a free build of the operating system with multiprocessor support.

```
Microsoft (R) Windows (R) 6.00. 6001 Service Pack 1 Multiprocessor Free.
```

If you know the processor type and the amount of physical memory installed on your computer, you can select the image name from one of the following. If you have a kernel debugger attached, you could also use the **lmv mnt** command to identify the original file name.

<span id="NTOSKRNL.EXE"></span>ntoskrnl.exe  
Uniprocessor x86 architecture systems with 4 GB of physical memory or less.

<span id="NTKRNLPA.EXE"></span>ntkrnlpa.exe  
Uniprocessor x86 architecture systems with PAE support.

<span id="NTKRNLMP.EXE"></span>ntkrnlmp.exe  
Multiprocessor x86 architecture systems with 4 GB of physical memory or less.

<span id="NTKRPAMP.EXE"></span>ntkrpamp.exe  
Multiprocessor x86 architecture systems with PAE support.

### <span id="step_2__copying_the_checked_files"></span><span id="STEP_2__COPYING_THE_CHECKED_FILES"></span> Step 2: Copying the Checked Files

Now that you know the names of the files that were used during your system installation, you can copy the checked versions of these files to your system. Find the files you have identified in the debug directory of the WDK or in the checked distribution kit. Then copy these files to the %SystemRoot%\\system32 directory of your system, giving them new, unique, file names. One way to ensure unique file names is to rename the file types from their original file types (.dll or .exe) to .chk when they are copied. Thus, using the example in Step 1, you would copy files from the checked distribution kit as follows:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">If the original file name in the debug directory of the WDK is:</th>
<th align="left">Copy it to the following file name in %SystemRoot%\system32:</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>ntkrnlmp.exe</p></td>
<td align="left"><p>ntkrnlmp.chk</p></td>
</tr>
<tr class="even">
<td align="left"><p>ntkrpamp.exe</p></td>
<td align="left"><p>ntkrpamp.chk</p></td>
</tr>
<tr class="odd">
<td align="left"><p>halmapic.dll</p></td>
<td align="left"><p>halmapic.chk</p></td>
</tr>
<tr class="even">
<td align="left"><p>hal.dll</p></td>
<td align="left"><p>hal.chk</p></td>
</tr>
</tbody>
</table>

 

### <span id="step_3__changing_the_boot_parameters_using_bcdedit"></span><span id="STEP_3__CHANGING_THE_BOOT_PARAMETERS_USING_BCDEDIT"></span>Step 3: Changing the boot parameters using BCDEdit

After you have copied the checked files to the %SystemRoot%\\system32 directory, you must create a boot-time entry that allows the system to start using these checked files. You can use BCDEdit to create this.

**Tip**   You can copy an existing boot entry to create a new boot entry that you can modify to use the checked operating system image and HAL. For example, to create a copy of the current boot entry, use the following command: **bcdedit /copy {current} /d "Windows 8.1 Partial Checked Build"**.
For general instructions on using BCDEdit, see [Tools for Changing Boot Options for Driver Testing and Debugging](boot-options-for-driver-testing-and-debugging.md) and [Editing Boot Options](editing-boot-options.md).

**Note**  Before setting BCDEdit options you might need to disable or suspend BitLocker and Secure Boot on the computer.

 

 

To configure a partial checked build on Windows Vista and later, use the [**BCDEdit /set**](https://msdn.microsoft.com/library/windows/hardware/ff542202) command and the **kernel** and **hal** options.

For example, the following commands configure a boot entry to use the checked versions of the kernel and HAL.

```
bcdedit /set {44a942bf-d6ee-11e3-baf8-000ffee4f6cd} kernel ntkrnlmp.chk
```

```
bcdedit /set {44a942bf-d6ee-11e3-baf8-000ffee4f6cd} hal hal.chk
```

You must also configure the computer for kernel debugging. Specifically, you must enable the boot entry for boot debugging ([**BCDEdit /bootdebug**](https://msdn.microsoft.com/library/windows/hardware/ff542183)). If you do not enable boot debugging and you do not have a kernel debugger connected to the computer, the computer will boot into the Windows Recover Environment if you select this new boot entry.

```
bcdedit /bootdebug {44a942bf-d6ee-11e3-baf8-000ffee4f6cd} on
```

To view the results of the commands, type **bcdedit /enum**. The **/enum** option lists all of the boot entries. For example, the following boot entry has been modified to use the checked versions of the kernel, and HAL. You must also enable the boot entry for boot debugging (bcdedit /bootdebug {ID} on).

```
## Windows Boot Loader
-------------------
identifier              {44a942bf-d6ee-11e3-baf8-000ffee4f6cd}
device                  partition=C:
path                    \Windows\system32\winload.exe
description             Windows 8.1 Partial Checked Build
locale                  en-US
inherit                 {bootloadersettings}
recoverysequence        {44a942bd-d6ee-11e3-baf8-000ffee4f6cd}
integrityservices       Disable
recoveryenabled         Yes
bootdebug               Yes
testsigning             Yes
allowedinmemorysettings 0x15000075
osdevice                partition=C:
systemroot              \Windows
kernel                  ntkrnlmp.chk
hal                     hal.chk
resumeobject            {44a942bb-d6ee-11e3-baf8-000ffee4f6cd}
nx                      OptIn
bootmenupolicy          Standard
debug                   Yes
```

### <span id="step_4__restart_the_computer"></span><span id="STEP_4__RESTART_THE_COMPUTER"></span>Step 4: Restart the computer

After you have made the changes, and have configured your computer for kernel debugging, enabled boot debugging, and have a kernel debugger connected. restart your computer. When you restart the computer, a new operating system boot option will be displayed that allows you to select your checked operating system image and HAL.

You can use the following procedure to verify that you are running the check build.

1.  Open the Event Viewer in the Computer Management Console (compmgmt.msc).

2.  Find the Event ID 6009 in the System Log.

    The properties for this event indicate whether you have the free or checked build of the operating system image installed.

For example, the following description for the event indicates a checked build of the operating system with multiprocessor support.

```
Microsoft (R) Windows (R) 6.00. 6001 Service Pack 1 Multiprocessor Checked.
```

## <span id="related_topics"></span>Related topics


[Setting Up Debugging (Kernel-Mode and User-Mode)](https://msdn.microsoft.com/library/windows/hardware/hh450944)

[Debugging Tools for Windows](https://msdn.microsoft.com/library/windows/hardware/ff551063)

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20[devtest\devtest]:%20Installing%20Just%20the%20Checked%20Operating%20System%20and%20HAL%20%28For%20Windows%20Vista%20and%20Later%29%20%20RELEASE:%20%2811/17/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")




