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

[Uplo
# Import Path and ifcopenshell
from pathlib import Path
import ifcopenshell

# Define modelname
modelname = "LLYN - ARK_modified"


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
        print(f"{name} = {value}")ading main.py…]()


