---
title: Build a cloud game development workstation
description: Learn how to customize your Game Development VM to build a cloud development workstation.
author: meaghanlewis
ms.topic: how-to
ms.date: 08/29/2022
ms.author: mosagie
ms.prod: azure-gaming
---

# Build your unique cloud game development workstation by customizing Game Dev VM

While the Game Development VM bundles many [common tools](/gaming/azure/game-dev-virtual-machine/tools-included-azure-game-dev-kit) used in game production, it’s  understandable that each game studio and individual game developer usually has their unique toolset as well. The Game Development VM is a starting point from which users build their customized cloud development workstation. That way, they don’t need to worry about the common tooling, and only focus on the delta that produces the desired outcome.

The high-level flow for producing a customized image based on the Game Development VM comprises of the following tasks:

1. Deploy a Game Dev VM into Azure.
1. Access the Game Dev VM and customize it.
1. Generalize Game Dev VM by using Sysprep.
1. Capture the customized Game Dev VM image.
1. Deploy the customized image and build your unique cloud workstation.

## Prerequisites

- An Azure account with an active subscription.
- Understand what Sysprep is and its process: [Sysprep (System Preparation) Overview](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)

## Step 1: Deploy a Game Dev VM into Azure

There are two main methods of deploying a Game Dev VM that you can choose from:

1. Using the Azure Marketplace/Portal UI as per the [tutorial on creating a Game Development Virtual Machine with Unreal Engine,](/gaming/azure/game-dev-virtual-machine/create-game-development-vm-for-unreal) or
1. Leverage ARM and CLI to deploy from the [Azure Quickstart Templates library](/gaming/azure/game-dev-virtual-machine/create-game-development-vm-arm-template).

**Note:** When deploying via the Azure Portal, you must check the box **VM will be used with sysprep to create a custom image** in the Advanced tab, as shown below. Otherwise, the following steps will fail.

:::image type="content" source="./media/build-cloud-workstation/sysprep-setting.png" alt-text="Screenshot showing how to enable the VM to be used with sysprep":::

## Step 2: Access the Game Dev VM and customize it

Here you can do whatever you need to build your own standard image. For example, but not limited to:

- Install and set up the software you or your team need to use, which are not included in the [list of tools](/gaming/azure/game-dev-virtual-machine/tools-included-azure-game-dev-kit) Game Dev VM already provides.
- Uninstall the software which you don’t need to free more space on the disk.
- Configure the operating system to the desired state you need. Update registry or group policy.

## Step 3: Generalize Game Dev VM by using Sysprep

After the customization of your own Game Dev VM is finished, you can follow the instructions to Sysprep the Game Dev VM: [Generalize a VM before creating an image](/azure/virtual-machines/generalize).

Sysprep removes user specific information so your customized Game Dev VM image can be used as a template to create multiple VMs in the future.

> [!WARNING]
> Once VM is generalized, it cannot be restarted. The process of generalizing a VM is not reversible. If you need to keep the original VM functioning, you should [create a copy](/azure/virtual-machines/windows/create-vm-specialized#option-3-copy-an-existing-azure-vm) of the VM and generalize its copy.

You can expect this pop-up window below after running the Sysprep command. After a few minutes, the VM will be shut down and you will lose the connection to the VM. These are all the expected results.

:::image type="content" source="./media/build-cloud-workstation/sysprep-working.png" alt-text="Screenshot showing that sysprep is working":::

> [!NOTE]
> Shortly after you lose the connection, the VM status on Azure portal will turn to Stopped. If not, please refresh the portal. You must wait the VM Status to be Stopped before moving into the next step. And please do NOT click the Stop button to manually change the VM status. Wait for the change to happen automatically.

:::image type="content" source="./media/build-cloud-workstation/stopped-status.png" alt-text="Screenshot showing the VM status turns to stopped":::

Once you see VM in the **Stopped** status, you can run the Azure PowerShell command _Set-AzVM_ in step 5 of how-to [generalize a VM before creating an image](/azure/virtual-machines/generalize) to set the status of the virtual machine to **Generalized**. Now you’re ready to move to the next step.

## Step 4: Capture the customized Game Dev VM image

The Game Dev VM image has been generalized, now you are ready to follow the instructions below to capture your own VM image. Please choose **Yes, share it to a gallery as an image version** in step 5 of the linked instructions, and you should see **Generalized** already be selected in step 8.

[Capture an image of a VM using the portal - Azure Virtual Machines](/azure/virtual-machines/capture-image-portal)

:::image type="content" source="./media/build-cloud-workstation/create-image.png" alt-text="Screenshot showing how to share gallery as an image and generalize VM during creation":::

After all the required information is filled out and validation passes, you can go ahead to create the image. It will take about 15 minutes for this deployment.

## Step 5: Deploy the customized image and build your unique cloud workstation

Now you have successfully captured the image and exported it to your Azure Compute Gallery, it’s time to start to use this image as a template to build one of your own game development cloud workstations.

Follow the steps documented in the article to [create a VM from your gallery](/azure/virtual-machines/vm-generalized-image-version?tabs=portal%2Ccli2), and you’ll be good to go. You can also use scripting like CLI or PowerShell to reference your customized image and automate the VM deployment process if you have multiple VMs to spin up at the same time.  

## Next steps

With your new Azure Game Dev VM up and running, you can [access the VM](/gaming/azure/game-dev-virtual-machine/create-game-development-vm-for-unreal) to verify the customization you made still exists. And get ready to deploy more identical cloud game development workstations by repeating Step 5 above.
