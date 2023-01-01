---
layout: page
title: "Chapter 0: Setup"
nav_order: 1
---
# Chapter 0: Setup
{: .no_toc }

In this section, you will import the base `Adventure Quest` project into Unity.
Before starting, you should ensure you have `Unity 2021.3.XX`. Although this
project may work with other versions, it has not been tested.

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

# 01. Get the Project Template

You can access the project template on GitHub here: [LINK](https://github.com/CaptainCoderOrg/AdventureQuest-Unity-2021/)

## Option 1 - Using GitHub (Recommended)

1. If you have a GitHub account, select `Use this Template` > `Create a new Repository`

![Use this Template](imgs/00%20-%20Setup/00-UseTemplate.png)

2. Give your repository a name. I recommend calling it `AdventureQuest`.
3. Click `Create repository from template`

![Create Repository](imgs/00%20-%20Setup/02-CreateRepo.png)

After a few second your copy of the template should be available. Next, you will
clone the repository. I've provided instructions below for using `GitHub
Desktop`. If you have a preferred way of cloning the repository, you can skip to
[02. Adding Project to Unity Hub](#02-adding-project-to-unity-hub)

1.  Click `Code` > `Open with GitHub Desktop`

![Open with GitHub Desktop](imgs/00%20-%20Setup/03-OpenWithGHD.png)

This should launch GitHub Desktop.

5. Take note of the `Local path` setting. This is where the repository will be
   cloned. Optionally you can change the `Local path` setting. 

6. Click `Clone`

![Click Clone](imgs/00%20-%20Setup/04-ClickClone.png)

After a few moments, the template will be cloned to your computer. 

## Creating a Development branch
{: .no_toc }

It is typically considered bad practice to work directly on your projects `main`
branch. Instead, your team will typically have a development branch that will
eventually be added to `main` when your project is ready to be released.

Before beginning, you should create a `development` branch. Below are instructions
to create a branch using **GitHub Desktop**:

{% include GitHub/NewBranch.md branch_name="development" %}

You are now
ready for [02. Adding Project to Unity Hub](#02-adding-project-to-unity-hub)

## Option 2 - Download Zip (Not Recommended)

{: .note }
If you use this option, your project will not be connected to a GitHub
repository and you will not be able to make incremental commits.

If you do not have a GitHub account, you can download the template as a ZIP by
clicking `Code` > `Download ZIP`:

![Download Zip](imgs/00%20-%20Setup/01-DownloadZip.png)


# 02. Adding Project to Unity Hub

**Note:** This guide was made using `Unity Hub 3.4.1`.

This project uses `Unity 2021.3.16f1`. Although this project *should* convert to
any version of `Unity 2021`, I highly recommend downloading this version for
compatibility.

1. Open Unity Hub
2. Click the `Projects` Tab
3. Click the arrow next to the `Open` button.
4. Click `Add project from disk`

![Add Project from Disk](imgs/00%20-%20Setup/05-AddProject.png)

5. Navigate to your repository and select the folder within called "Adventure
   Quest". This folder should contain a folder called `Assets`.

![Select Folder](imgs/00%20-%20Setup/SelectFolder.gif)

6. The `Adventure Quest` project should now appear in Unity Hub. Double Click to open it.

![Open Project](imgs/00%20-%20Setup/07-OpenProject.png)

The first time the project is opened, it will take several minutes to load. 

# Good Time to Commit

Now would be a good time to make a `git` commit. You just finished a **chore**.
More specifically, you just initialized the Adventure Quest Unity project.

{% include GitHub/CreateCommit.md %}

# What's next?

With the project setup and ready to go, you can proceed to [Part 1: Modeling a Die].

---

[Part 1: Modeling a Die]: {% link pages/01-CreatingADieRoller/00-Part1-ModelingADie.md %}