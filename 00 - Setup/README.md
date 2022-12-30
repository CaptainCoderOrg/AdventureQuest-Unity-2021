# Setup

## 01. Get the Project Template

You can access the project template on GitHub here: [LINK](https://github.com/jcollard/AdventureQuest-Unity-2021)

### Option 1 - Using GitHub (Recommended)

1. If you have a GitHub account, select `Use this Template` > `Create a new Repository`

![Use this Template](../imgs/00%20-%20Setup/00-UseTemplate.png)

2. Give your repository a name. I recommend calling it `AdventureQuest`.
3. Click `Create repository from template`

![Create Repository](../imgs/00%20-%20Setup/02-CreateRepo.png)

After a few second your copy of the template should be available. Next, you will
clone the repository. I've provided instructions below for using `GitHub
Desktop`. If you have a preferred way of cloning the repository, you can skip to
[02. Adding Project to Unity Hub](#02-adding-project-to-unity-hub)

4.  Click `Code` > `Open with GitHub Desktop`

![Open with GitHub Desktop](../imgs/00%20-%20Setup/03-OpenWithGHD.png)

This should launch GitHub Desktop.

5. Take note of the `Local path` setting. This is where the repository will be
   cloned. Optionally you can change the `Local path` setting. 

6. Click `Clone`

![Click Clone](../imgs/00%20-%20Setup/04-ClickClone.png)

After a few moments, the template will be cloned to your computer. You are now
ready for [02. Adding Project to Unity Hub](#02-adding-project-to-unity-hub)

### Option 2 - Download Zip (Not Recommended)

**Note:** If you use this option, your project will not be connected to a GitHub
repository and you will not be able to make incremental commits.

If you do not have a GitHub account, you can download the template as a ZIP by
clicking `Code` > `Download ZIP`:

![Download Zip](../imgs/00%20-%20Setup/01-DownloadZip.png)


## 02. Adding Project to Unity Hub

**Note:** This guide was made using `Unity Hub 3.4.1`.

This project uses `Unity 2021.3.16f1`. Although this project *should* convert to
any version of `Unity 2021`, I highly recommend downloading this version for
compatibility.

1. Open Unity Hub
2. Click the `Projects` Tab
3. Click the arrow next to the `Open` button.
4. Click `Add project from disk`

![Add Project from Disk](../imgs/00%20-%20Setup/05-AddProject.png)

5. Navigate to your repository and select the folder within called "Adventure
   Quest". This folder should contain a folder called `Assets`.

![Select Folder](../imgs/00%20-%20Setup/SelectFolder.gif)

6. The `Adventure Quest` project should now appear in Unity Hub. Double Click to open it.

![Open Project](../imgs/00%20-%20Setup/07-OpenProject.png)

The first time the project is opened, it will take several minutes to load. If all went well,
you are now ready to proceed to [Part 1: Modeling a Die](../01%20-%20Modeling%20a%20Die/README.md).

