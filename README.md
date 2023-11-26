# 41934 Advanced BIM - A4: OpenBIM Guru

## Group 7
|Name|Study Number|
|----|------------|
|Emil Hjort Kristensen|S193924|
|Rasmus Olsen|S184506|

#### Focus area: LCA

#### Role: Analyst Level 1

## Focus area and BIM use
Our use case involves creating a tool capable of extracting data such as materials, quantities, and EPDs from a BIM model. The tool then compiles the data into a .json file that can be directly imported into LCAByg. The file contains all the relevant information so a sustainability engineer can initiate a building LCA. 

In short, the goal of the tool is described as the following:
Streamline the process of conducting a building LCA from a BIM model by compiling all the necessary data a sustainability engineer needs. 

The tool was created entirely within Blender, utilizing the program's scripting prompt and the BlenderBim add-on. The script was developed exclusively in the Python programming language. 

## Role identification
Our tool is specifically designed for utilization by the role of “Analyst Level 1” since its primary objective is to analyze a standard IFC file and extract essential LCA data. This data would be presented in a comprehensive Excel-sheet overview, aiding sustainability engineers in their analysis and decision-making processes. However, due to its current limited LOD, the tool remains at Level 1.

# Tutorials
## Tutorial 1 - Extraction of wall data
Before running the script, make sure that your IFC model has quantities added. The following link will take you to a separate guide that will show you how it is done in BlenderBim:

https://github.com/timmcginley/41934/blob/main/Concepts/BlenderBIM/AddQuantitiesToIfcModelInBlenderBIM/README.md

As a precursor for the script to work you need to make a folder structure with a folder called “model” where you save your IFC file. Outside of the folder, you can have your script saved. 

When done correctly, your folder structure would look something like this: 
<img width="676" alt="BIM_A4_1" src="https://github.com/Emilhjort/A4-OpenBIM-Guru/assets/145363406/06632d78-088f-4554-9259-e9324fef26f8">




