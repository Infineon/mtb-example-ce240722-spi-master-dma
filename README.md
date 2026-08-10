# PSOC&trade; Control C3 MCU: SCB SPI master with DMA

This example demonstrates the use of the SPI Serial Communication Block (SCB) resource for PSOC&trade; Control C3 MCU in master mode using DMA. The SPI master is configured to send command packets to control the user LED on the slave. Each supported kit has two dedicated SPI SCBs — one as master and one as slave — so a single board is sufficient for this code example by default. Optionally, two separate boards can be used by programming one with `SPI_MODE_MASTER` and the other with `SPI_MODE_SLAVE`.


[View this README on GitHub.](https://github.com/Infineon/mtb-example-ce240722-spi-master-dma)

[Provide feedback on this code example.](https://yourvoice.infineon.com/jfe/form/SV_1NTns53sK2yiljn?Q_EED=eyJVbmlxdWUgRG9jIElkIjoiQ0UyNDA3MjIiLCJTcGVjIE51bWJlciI6IjAwMi00MDcyMiIsIkRvYyBUaXRsZSI6IlBTT0MmdHJhZGU7IENvbnRyb2wgQzMgTUNVOiBTQ0IgU1BJIG1hc3RlciB3aXRoIERNQSIsInJpZCI6ImJhbGFjaGFuZGVyLnN1YnJhbWFuaXlhcGlsbGFpa2FubmFuQGluZmluZW9uLmNvbSIsIkRvYyB2ZXJzaW9uIjoiMS4yLjAiLCJEb2MgTGFuZ3VhZ2UiOiJFbmdsaXNoIiwiRG9jIERpdmlzaW9uIjoiTUNEIiwiRG9jIEJVIjoiSUNXIiwiRG9jIEZhbWlseSI6IlBTT0MifQ==)


## Requirements

- [ModusToolbox&trade;](https://www.infineon.com/modustoolbox) v3.8 or later
- Board support package (BSP) minimum required version for:
   - KIT_PSC3M5_EVK: v1.0.3
   - KIT_PSC3M5_CC1: v1.0.3
   - KIT_PSC3M5_CC2: v1.0.3
   - KIT_PSC3M6_EVAL: v1.0.0
- Programming language: C
- Associated parts: PSOC&trade; Control C3 MCU Entry Line and PSOC&trade; Control C3 MCU Main Line parts


## Supported toolchains (make variable 'TOOLCHAIN')

- GNU Arm&reg; Embedded Compiler v14.2.1 (`GCC_ARM`) – Default value of `TOOLCHAIN`
- Arm&reg; Compiler v6.22 (`ARM`)
- IAR C/C++ Compiler v9.70.4 (`IAR`)

## Supported kits (make variable 'TARGET')

- [PSOC&trade; Control C3M5 Evaluation Kit](https://www.infineon.com/evaluation-board/KIT-PSC3M5-EVK) (`KIT_PSC3M5_EVK`) – Default value of `TARGET`
- [PSOC&trade; Control C3M5 Digital Power Control Card](https://www.infineon.com/evaluation-board/KIT-PSC3M5-CC1) (`KIT_PSC3M5_CC1`)
- [PSOC&trade; Control C3M5 Motor Drive Control Card](https://www.infineon.com/evaluation-board/KIT-PSC3M5-CC2) (`KIT_PSC3M5_CC2`)
- [PSOC&trade; Control C3M6 Evaluation Kit](https://www.infineon.com/evaluation-board/KIT-PSC3M6-EVAL) (`KIT_PSC3M6_EVAL`)


## Hardware setup

This example uses the board's default configuration. See the kit user guide to ensure that the board is configured correctly.

This example supports two operation modes:

- **Single kit (`SPI_MODE_BOTH` — default):** Both the master and slave SCBs run on the same device. Connect the master and slave SPI pins together using jumper wires as shown in Table 1. All supported kits have two dedicated SPI SCBs, so no additional hardware is needed.

- **Two kits (`SPI_MODE_MASTER` and `SPI_MODE_SLAVE`):** Program one kit with `SPI_MODE_MASTER` and a second kit with `SPI_MODE_SLAVE`. Connect the corresponding master pins on kit 1 to the slave pins on kit 2 using Table 1 (Master SCLK → Slave SCLK, Master MOSI → Slave MOSI, Master MISO → Slave MISO, Master SS0 → Slave SS0). Also connect the grounds of both kits together.

For each kit, verify with the corresponding custom *design.modus* file to confirm the SPI pin assignments.

**Table 1. SPI master and slave pin assignments**

 Board name               | Master SCLK | Master MOSI | Master MISO | Master SS0 | Slave SCLK | Slave MOSI | Slave MISO | Slave SS0
 ------------------------| ----------  | ----------  | ---------   | ---------  | ---------- | ---------- | ---------- | ---------
 KIT_PSC3M5_EVK          | 7.0         | 7.1         | 7.2         | 7.3        | 3.2        | 3.0        | 4.1        | 3.3
 KIT_PSC3M5_CC1          | 7.0         | 7.1         | 7.2         | 7.3        | 3.2        | 3.0        | 4.1        | 3.3
 KIT_PSC3M5_CC2          | 5.2         | 5.0         | 5.1         | 5.3        | 3.2        | 3.0        | 4.1        | 3.3
 KIT_PSC3M6_EVAL         | 7.0         | 7.1         | 7.2         | 7.3        | 3.2        | 3.0        | 4.1        | 3.3


## Software setup

See the [ModusToolbox&trade; tools package installation guide](https://www.infineon.com/ModusToolboxInstallguide) for information about installing and configuring the tools package.

This example requires no additional software or tools.


## Using the code example


### Create the project

The ModusToolbox&trade; tools package provides the Project Creator as both a GUI tool and a command line tool.

<details><summary><b>Use Project Creator GUI</b></summary>

1. Open the Project Creator GUI tool

   There are several ways to do this, including launching it from the dashboard or from inside the Eclipse IDE. For more details, see the [Project Creator user guide](https://www.infineon.com/ModusToolboxProjectCreator) (locally available at *{ModusToolbox&trade; install directory}/tools_{version}/project-creator/docs/project-creator.pdf*)

2. On the **Choose Board Support Package (BSP)** page, select a kit supported by this code example. See [Supported kits](#supported-kits-make-variable-target)

   > **Note:** To use this code example for a kit not listed here, you may need to update the source files. If the kit does not have the required resources, the application may not work

3. On the **Select Application** page:

   a. Select the **Application(s) Root Path** and the **Target IDE**

      > **Note:** Depending on how you open the Project Creator tool, these fields may be pre-selected for you

   b. Select this code example from the list by enabling its check box

      > **Note:** You can narrow the list of displayed examples by typing in the filter box

   c. (Optional) Change the suggested **New Application Name** and **New BSP Name**

   d. Click **Create** to complete the application creation process

</details>


<details><summary><b>Use Project Creator CLI</b></summary>

The 'project-creator-cli' tool can be used to create applications from a CLI terminal or from within batch files or shell scripts. This tool is available in the *{ModusToolbox&trade; install directory}/tools_{version}/project-creator/* directory.

Use a CLI terminal to invoke the 'project-creator-cli' tool. On Windows, use the command-line 'modus-shell' program provided in the ModusToolbox&trade; installation instead of a standard Windows command-line application. This shell provides access to all ModusToolbox&trade; tools. You can access it by typing "modus-shell" in the search box in the Windows menu. In Linux and macOS, you can use any terminal application.

The following example clones the "[PSOC&trade; Control C3 MCU: SCB SPI master with DMA](https://github.com/Infineon/mtb-example-ce240722-spi-master-dma)" application with the desired name "SCBSPIMasterDMA" configured for the *KIT_PSC3M5_EVK* BSP into the specified working directory, *C:/mtb_projects*:

   ```
   project-creator-cli --board-id KIT_PSC3M5_EVK --app-id mtb-example-ce240722-spi-master-dma --user-app-name SCBSPIMasterDMA --target-dir "C:/mtb_projects"
   ```

The 'project-creator-cli' tool has the following arguments:

Argument | Description | Required/optional
---------|-------------|-----------
`--board-id` | Defined in the <id> field of the [BSP](https://github.com/Infineon?q=bsp-manifest&type=&language=&sort=) manifest | Required
`--app-id`   | Defined in the <id> field of the [CE](https://github.com/Infineon?q=ce-manifest&type=&language=&sort=) manifest | Required
`--target-dir`| Specify the directory in which the application is to be created if you prefer not to use the default current working directory | Optional
`--user-app-name`| Specify the name of the application if you prefer to have a name other than the example's default name | Optional

<br>

> **Note:** The project-creator-cli tool uses the `git clone` and `make getlibs` commands to fetch the repository and import the required libraries. For details, see the "Project creator tools" section of the [ModusToolbox&trade; tools package user guide](https://www.infineon.com/ModusToolboxUserGuide) (locally available at {ModusToolbox&trade; install directory}/docs_{version}/mtb_user_guide.pdf).

</details>


### Open the project

After the project has been created, you can open it in your preferred development environment.


<details><summary><b>Eclipse IDE</b></summary>

If you opened the Project Creator tool from the included Eclipse IDE, the project will open in Eclipse automatically.

For more details, see the [Eclipse IDE for ModusToolbox&trade; user guide](https://www.infineon.com/MTBEclipseIDEUserGuide) (locally available at *{ModusToolbox&trade; install directory}/docs_{version}/mt_ide_user_guide.pdf*).

</details>


<details><summary><b>Visual Studio (VS) Code</b></summary>

Launch VS Code manually, and then open the generated *{project-name}.code-workspace* file located in the project directory.

For more details, see the [Visual Studio Code for ModusToolbox&trade; user guide](https://www.infineon.com/MTBVSCodeUserGuide) (locally available at *{ModusToolbox&trade; install directory}/docs_{version}/mt_vscode_user_guide.pdf*).

</details>


<details><summary><b>Arm&reg; Keil&reg; µVision&reg;</b></summary>

Double-click the generated *{project-name}.cprj* file to launch the Keil&reg; µVision&reg; IDE.

For more details, see the [Arm&reg; Keil&reg; µVision&reg; for ModusToolbox&trade; user guide](https://www.infineon.com/MTBuVisionUserGuide) (locally available at *{ModusToolbox&trade; install directory}/docs_{version}/mt_uvision_user_guide.pdf*).

</details>


<details><summary><b>IAR Embedded Workbench</b></summary>

Open IAR Embedded Workbench manually, and create a new project. Then select the generated *{project-name}.ipcf* file located in the project directory.

For more details, see the [IAR Embedded Workbench for ModusToolbox&trade; user guide](https://www.infineon.com/MTBIARUserGuide) (locally available at *{ModusToolbox&trade; install directory}/docs_{version}/mt_iar_user_guide.pdf*).

</details>


<details><summary><b>Command line</b></summary>

If you prefer to use the CLI, open the appropriate terminal, and navigate to the project directory. On Windows, use the command-line 'modus-shell' program; on Linux and macOS, you can use any terminal application. From there, you can run various `make` commands.

For more details, see the [ModusToolbox&trade; tools package user guide](https://www.infineon.com/ModusToolboxUserGuide) (locally available at *{ModusToolbox&trade; install directory}/docs_{version}/mtb_user_guide.pdf*).

</details>


## Operation

You can configure this example to work in master-only, slave-only, or both master and slave SPI modes by setting the `SPI_MODE` macro in the *interface.h* file. By default (`SPI_MODE_BOTH`), both the master and slave SCBs run on a single kit. To use two separate kits, program one with `SPI_MODE_MASTER` and the other with `SPI_MODE_SLAVE`. See [Hardware setup](#hardware-setup) for the required connections.

1. Connect the board to your PC using the provided USB cable through the Debug USB connector on the board

2. Program the board using one of the following:

   <details><summary><b>Using Eclipse IDE</b></summary>

      1. Select the application project in the Project Explorer

      2. In the **Quick Panel**, scroll down, and click **\<Application Name> Program (KitProg3_MiniProg4)** or **\<Application Name> Program (JLink)**
   </details>


   <details><summary><b>In other IDEs</b></summary>

   Follow the instructions in your preferred IDE.

   </details>


   <details><summary><b>Using CLI</b></summary>

     From the terminal, execute the `make program` command to build and program the application using the default toolchain to the default target. The default toolchain is specified in the application's Makefile but you can override this value manually:
      ```
      make program TOOLCHAIN=<toolchain>
      ```

      Example:
      ```
      make program TOOLCHAIN=GCC_ARM
      ```
   </details>

3. After programming, the application starts automatically. Observe that the kit LED blinks at 1 Hz (the master toggles the LED ON/OFF command every 1 second; when using two kits, observe the LED on the slave kit)


## Debugging

You can debug the example to step through the code.


<details><summary><b>In Eclipse IDE</b></summary>

Use the **\<Application Name> Debug (KitProg3_MiniProg4)** or **\<Application Name> Debug (JLink)** configuration in the **Quick Panel**. For details, see the "Program and debug" section in the [Eclipse IDE for ModusToolbox&trade; user guide](https://www.infineon.com/MTBEclipseIDEUserGuide).


</details>


<details><summary><b>In other IDEs</b></summary>

Follow the instructions in your preferred IDE.

</details>


## Design and implementation


### Resources and settings

The Arm&reg; Cortex&reg;-M33 (CM33) CPU controls both the master and slave SCBs. All supported kits have two dedicated SPI SCBs, allowing this example to run on a single kit without any additional hardware.

The master sends a packet to the slave with a command to turn ON or turn OFF the user LED. The packets are sent at an interval of 1 second. DMA is used to transfer the command data from the SRAM to the SPI FIFO at the master side, and similarly from the SPI FIFO to the SRAM at the slave side. The slave receives the packet and controls the LED according to the command.

**Table 2. Application resources**

 Resource  |    Alias/object     |    Purpose
 :-------- | :------------------ | :----------
 SCB (SPI) |      mSPI      | Master SPI SCB
 SCB (SPI) |      sSPI      | Slave SPI SCB
 GPIO   |     CYBSP_USER_LED   | LED indication
 DMA    |     txDma      | Data transfer
 DMA    |     rxDma      | Data transfer

<br>


## Related resources

Resources  | Links
-----------|----------------------------------
Documentation | [PSOC&trade; Control C3 MCU documents](https://documentation.infineon.com/psoccontrolc3/docs/kfc1732622054982)
Development kits | [PSOC&trade; Control C3 development kits](https://documentation.infineon.com/psoccontrolc3/docs/yyw1732688626489)
Tools, BSPs, libraries, and code examples | [ModusToolbox&trade;](https://documentation.infineon.com/modustoolbox/) – ModusToolbox&trade; software is a collection of easy-to-use libraries and tools enabling rapid development with Infineon MCUs for applications ranging from wireless and cloud-connected systems, edge AI/ML, embedded sense and control, to wired USB connectivity using PSOC&trade; Industrial/IoT MCUs, AIROC&trade; Wi-Fi and Bluetooth&reg; connectivity devices, XMC&trade; Industrial MCUs, and EZ-USB&trade;/EZ-PD&trade; wired connectivity controllers. ModusToolbox&trade; incorporates a comprehensive set of BSPs, HAL, libraries, configuration tools, and provides support for industry-standard IDEs to fast-track your embedded application development

<br>


## Other resources

Infineon provides a wealth of data at [www.infineon.com](https://www.infineon.com) to help you select the right device, and quickly and effectively integrate it into your design.


## Document history

Document title: *CE240722* - *PSOC&trade; Control C3 MCU: SCB SPI master with DMA*

 Version | Description of change
 ------- | ---------------------
 1.0.0   | New code example 
 1.1.0   | Added support for KIT_PSC3M5_CC2 and KIT_PSC3M5_CC1
 1.2.0   | Added KIT_PSC3M6_EVAL support
<br>


All referenced product or service names and trademarks are the property of their respective owners.

The Bluetooth&reg; word mark and logos are registered trademarks owned by Bluetooth SIG, Inc., and any use of such marks by Infineon is under license.

PSOC&trade;, formerly known as PSoC&trade;, is a trademark of Infineon Technologies. Any references to PSoC&trade; in this document or others shall be deemed to refer to PSOC&trade;.

---------------------------------------------------------

(c) 2026, Infineon Technologies AG, or an affiliate of Infineon Technologies AG. All rights reserved.
This software, associated documentation and materials ("Software") is owned by Infineon Technologies AG or one of its affiliates ("Infineon") and is protected by and subject to worldwide patent protection, worldwide copyright laws, and international treaty provisions. Therefore, you may use this Software only as provided in the license agreement accompanying the software package from which you obtained this Software. If no license agreement applies, then any use, reproduction, modification, translation, or compilation of this Software is prohibited without the express written permission of Infineon.
<br>
Disclaimer: UNLESS OTHERWISE EXPRESSLY AGREED WITH INFINEON, THIS SOFTWARE IS PROVIDED AS-IS, WITH NO WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING, BUT NOT LIMITED TO, ALL WARRANTIES OF NON-INFRINGEMENT OF THIRD-PARTY RIGHTS AND IMPLIED WARRANTIES SUCH AS WARRANTIES OF FITNESS FOR A SPECIFIC USE/PURPOSE OR MERCHANTABILITY. Infineon reserves the right to make changes to the Software without notice. You are responsible for properly designing, programming, and testing the functionality and safety of your intended application of the Software, as well as complying with any legal requirements related to its use. Infineon does not guarantee that the Software will be free from intrusion, data theft or loss, or other breaches (“Security Breaches”), and Infineon shall have no liability arising out of any Security Breaches. Unless otherwise explicitly approved by Infineon, the Software may not be used in any application where a failure of the Product or any consequences of the use thereof can reasonably be expected to result in personal injury.
