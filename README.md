# Solving-Virtualized-Intel-VT-x-EPT-is-not-supported-on-this-platform-GNS3-Issue
In this post I share my solution to the common issue with GNS3 VM in the new Windows update (24H2) : **"Virtualized Intel VT-x/EPT is not supported on this platform"**.

![Intro](https://preview.redd.it/eclrryr4bc081.png?width=320&format=png&auto=webp&s=33dedf99a4ea8960b31e5b98fb0847d8c136b9ef)

## Preface
After two weeks spent trying to find the solution, no help from internet was able to solve my issue and that was because it was a problem
solely with the new Windows update from 2024 (the 24H2 update). Looking in that direction, I managed to find a solution!


In Windows this error happens with VMWare Workstation when Virtualization-based security is enabled (If you go to the System Information app in your Windows and the
**Virtualization-based security** is something different than **Not enabled**, than probably this is the cause of the issue). And if you
search the net, you will find many articles and videos solving with similar steps to each other this problem, but when you try them,
they don't seem to work and that is because of the big changes Windows has made in it's update (the 24H2 update). So in this repo, I will
show you the steps, you have to make to disable the Virtualization-based security and by doing that, solve the issue. The first steps
are the common steps that may or may not work for you (those steps that you'll find in many places in Internet) and the next steps are the
Windows-version-specific steps, that will most likely solve the issue.

![Windows 24H2](https://www.restorex360.com/wp-content/uploads/2024/09/Windows-11-24H2.webp)

## NO MATTER IF YOU WANT TO SOLVE THIS ISSUE FOR GNS3 OR JUST WANT TO DISABLE VIRTUALIZATION-BASED SECURITY (SOLVE IT FOR SOME OTHER VM) YOU CAN FOLLOW THIS GUIDE, AS IT IS NOT PROGRAM SPECIFIC!



## Solving Steps:
 1.
      -  1. In search box type <em>"gpedit"</em> then Goto  - Computer Configuration - Administrative Templates - System - Device Guard - Turn on Virtualization Based Security. Now Double click that and "Disable"

      -  2. In search box, type <em>Turn Windows features on or off</em>, now uncheck <em>Hyper-V</em> and restart system.

      - 3. Open Registry Editor by typing <en>regedit</en> in search box, now Go to <en>HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\DeviceGuard</en>. Add a new DWORD value named **<en>EnableVirtualizationBasedSecurity<en>** and set it to **0** to disable it.
        Next Go to HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA.  Add a new DWORD value named LsaCfgFlags and set it to 0 to disable it.

      - 4. Open command prompt as a administrator and type the following commands  
      ```bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader```

      - 5. Then copy paste the rest below and press enter
     ```bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"```
      ```bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}```
      ```bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS```
      ```bcdedit /set hypervisorlaunchtype off```
      - 6. Now, Restart your system
        
      **The steps are from a video of Britec09, which you can watch here: https://www.youtube.com/watch?v=VIBdY-5zr58**.
      **I coppied his video in my repo so you can watch it there if it is not available for some reason on Youtube.**


2. Disable all services in the Services app on Windows that contain 'Hyper-V'. You also need to disable the 'HV Host Service'. Disabling the services is done by clicking on them and choosing 'Disable' from the dropdown menu.

   
4. Then follow this article: **Disable Virtualization based security*** (https://simpleoracle.com/2024/10/09/how-to-disable-windows-virtualization-device-based-security-vbs/). Try all the steps in the article. the first steps are the old methods and the next steps are the new method. IMPORTANT: When using the Microsoft tool from the article, you can't disable Virt. Based Security, Device Guard, Credential Guard if the Secure Boot option in BIOS is disabled! Play with that setting and it may work.

**I have copied the article in my repo so you can access-it even if it is not available anymore for some reason.**

## You can find my original post in the GNS3 Community here: https://gns3.com/virtualized-intel-vt-x-ept-is-not-supported-on-this-platform. My profile name is Martin Mihalkov.
