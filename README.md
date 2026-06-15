**Work in Progress**

![ModelNova Fusion Studio](./Documentation/image/ModelNova.png "ModelNova Fusion Studio")

This repository shows how to build **Edge AI applications with Cortex-M/Ethos-U-based microcontrollers**. It uses the [SDS-Framework](https://www.keil.arm.com/packs/sds-arm) for data capturing and testing, [ModelNova - Fusion Studio](https://modelnova.ai/fusion-studio-beta) for AI model development, and [Keil MDK](https://www.keil.arm.com/) for embedded development.

> [!TIP]
> Register for [Webinar 3: Integrating ModelNova Fusion Studio with Arm Keil MDK](https://developer.arm.com/embedded-and-microcontrollers/modelnova-fusion-studio-with-keil-mdk) to learn more.

## Quick Start

The [RockPaperScissors (RPS)](./Documentation/README.md) project implements an AI model that detects [three hand gestures](https://en.wikipedia.org/wiki/Rock_paper_scissors) ([RPS_cls_dataset](./RockPaperScissors/RPS_cls_dataset/) provides test data). The [`AppKit-E8_USB/SDS.csolution.yml`](./RockPaperScissors/AppKit-E8_USB/SDS.csolution.yml) project uses the SDS framework for testing the AI model on the Alif AppKit-E8 hardware or an Arm FVP simulation model.

### Keil MDK

1. Install [Keil Studio for VS Code](https://marketplace.visualstudio.com/items?itemName=Arm.keil-studio-pack) and [Arm SDS for VS Code](https://marketplace.visualstudio.com/items?itemName=Arm.cmsis-sds) from the VS Code marketplace.
2. Clone this repository (for example using [Git in VS Code](https://code.visualstudio.com/docs/sourcecontrol/intro-to-git)) or download the ZIP file. Then open the base folder in VS Code.
3. Open the [CMSIS View](https://mdk-packs.github.io/vscode-cmsis-solution-docs/userinterface.html#2-main-area-of-the-cmsis-view) in VS Code, use *Open Solution in Workspace* (... menu), and choose `RockPaperScissors/AppKit-E8_USB/SDS.csolution.yml` to open the project.
4. The related tools and software packs are downloaded and installed. Review progress with *View - Output - CMSIS Solution*.
5. In the CMSIS view, use the [Action buttons](https://github.com/ARM-software/vscode-cmsis-csolution?tab=readme-ov-file#action-buttons) to build, load, and debug the example to the [target hardware](./Documentation/README.md#target-hardware).

> [!TIP]
> If you are new to Alif devices and boards, start with the `Blink_HP` example project using *Create Solution* with the board `Alif AppKit-E8-AIML`.

## Application

The diagram below illustrates the RPS application architecture. During algorithm development, the [SDS-Framework](https://www.keil.arm.com/packs/sds-arm) supports recording and playback of data streams for analysis and ML model training.

![Application Structure](./Documentation/image/Application-Structure.png "Application Structure")

### Development Steps

**Create Classic Embedded Application:**

1. [Create the input interface](./Documentation/README.md#input-interface-and-signal-conditioning), add signal conditioning, and start capturing data for ML model training.
2. [Select an ML model](./Documentation/README.md#create-ml-model), then use the captured data for training, analysis, and creation of the optimized ML model.
3. [Integrate the ML model](./Documentation/README.md#integrate-ml-model) into the SDS framework and analyze performance.

**Test Embedded Application:**

1. [Arm SDS for VS Code](https://marketplace.visualstudio.com/items?itemName=Arm.cmsis-sds) lets you capture the various data streams of the application in [SDS data files](https://arm-software.github.io/SDS-Framework/main/overview.html) for analysis.
    ![SDS Data View](./Documentation/image/Arm_SDS.png)
2. The [SDS.sdsio.yml](./RockPaperScissors/AppKit-E8_USB/SDS.sdsio.yml) configuration file defines `play:` steps for regression testing for example with [Contiguous Integration (CI)](#contiguous-integration-ci)

**Create ML Model:**

1. [Capture new data](./Documentation/README.md#capture-new-data) where the ML model does not deliver the expected results.
2. [Retrain the AI model](./Documentation/README.md#retrain-ai-model) using additional training data to optimize performance.
3. [Add regression testing](./Documentation/README.md#regression-test) before integrating a new AI model into the embedded system.

### ModelNova Fusion Studio

1. Download and install [ModelNova Fusion Studio](https://modelnova.ai/fusion-studio-beta).
2. Launch the application and login to the application using the PAT from the **Get it from here** button

    ![ModelNova Fusion Studio Launch Screen](./Documentation/image/login_screen.png "Login Screen")

3. Click the **New Workspace** button.
4. Choose to create any type of workspace (e.g., from scratch, starter pack, etc.).

    ![New Workspace](./Documentation/image/Create_Workspace_1.png)

5. Select the appropriate **Category** and **Domain** for your ML project.

    ![New Workspace](./Documentation/image/Create_Workspace_2.png)

6. Enter the workspace to begin your workflow.

    While Fusion Studio supports multiple workspace creation methods (Scratch, Starter Pack, AI Assist), the **ML workflow remains identical for all workspaces**.

## Test with SystemView

## Contiguous Integration (CI)

This repository uses [CI Workflows](https://github.com/Arm-Examples/.github/blob/main/profile/CICD.md) listed below to build artifacts and verify projects. Examples are verified with the build system of Keil Studio that uses the CMSIS-Toolbox and CMake. Using this toolchain supports CI with:

Tool installation based on a single vcpkg-configuration.json file for desktop and CI environments.
CMSIS solution files (*.csolution.yml) that enable seamless builds in CI, for example using GitHub actions.

CI Workflow                                                          | Description
:--------------------------------------------------------------------|:---------------------------------------
[Test_RPS_SSE-320-U85](./.github/workflows/Test_RPS_SSE-320-U85.yml) | Build and run image with SDS data input on FVP simulation model
[Build_RPS_AppKit-E8](./.github/workflows/Build_RPS_AppKit-E8.yml)   | Build image with SystemView enabled for testing on hardware
[Run_RPS_AppKit-E8](./.github/workflows/Run_RPS_AppKit-E8.yml)       | Run image on hardware with SDSIO-Server and SystemView for timing analysis
[Build_Variants](./.github/workflows/Build_Variants.yml)             | Ensure that everything builds; Build the different context variants: `project.build-type+target-type`

## Issues or Questions

Use the [**Issues**](./issues) tab to raise questions or issues.
