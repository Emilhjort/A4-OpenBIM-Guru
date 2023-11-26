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

After successfully completing the previous steps, the subsequent phase involves the execution of the following Python code for data extraction. Specifically, this code is designed to extract NetSideArea and NetVolume data for all walls in the IFC model.

Snapshot of the code:
```
# Import Path and ifcopenshell
from pathlib import Path
import ifcopenshell

# Define modelname
modelname = "Name_of_IFC_model"


# Search for filepath and check if model folder exists and run script on model 
try:
    dir_path = Path(__file__).parent
    model_url = Path.joinpath(dir_path, 'model', modelname).with_suffix('.ifc')
    model = ifcopenshell.open(model_url)
except OSError:
    try:
        import bpy
        model_url = Path.joinpath(Path(bpy.context.space_data.text.filepath).parent, 'model', modelname).with_suffix('.ifc')
        model = ifcopenshell.open(model_url)
    except OSError:
        print(f"ERROR: please check your model folder : {model_url} does not exist")

# Get all walls in the IFC file
walls = model.by_type("IfcWall")

# Create a dictionary to store the wall type properties
wall_type_properties = {}

# Iterate over each wall and print its properties
for wall in walls:
    wall_type = ifcopenshell.util.element.get_type(wall)

    # Get selected properties for Walls
    Wall_qtos=(ifcopenshell.util.element.get_psets(wall, qtos_only=True))
    
    # Create a dictionary to map the quantity names to their respective values
    quantity_names = {
        "NetSideArea": Wall_qtos["Qto_WallBaseQuantities"]["NetSideArea"],
        "NetVolume": Wall_qtos["Qto_WallBaseQuantities"]["NetVolume"],
    }

    # Add the wall type properties to the dictionary
    if wall_type.Name not in wall_type_properties:
        wall_type_properties[wall_type.Name] = quantity_names
    else:
        for name, value in quantity_names.items():
            wall_type_properties[wall_type.Name][name] += value

# Print the final list of wall type properties
for wall_type, properties in wall_type_properties.items():
    print(f"Wall Type: {wall_type}")
    for name, value in properties.items():
        print(f"{name} = {value}")
```
Copy the code into Blender 3.6 and run the code. In the window of Toggle System Console a list of every WallType and the associated NetSideArea and NetVolume will be displayed.

Example of output:
<img width="546" alt="BIM_A4_2" src="https://github.com/Emilhjort/A4-OpenBIM-Guru/assets/145363406/6a295bd5-01bb-4b5b-bc67-d65741b118ae">

This tool is meant to undergo further development for the extraction of multiple properties; however, the current code represents a preliminary draft for this ongoing development.


