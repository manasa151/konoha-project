# **Table of content:**
1. [Create Tank](#create-tank)
1. [Display Tank Statistics](#display-tank-statistics)
1. [Store tank in cargo manifest](#store-tank-in-cargo-manifest)
1. [Blender Settings](#blender-settings)
1. [Syntax](#syntax)



## Create Tank
##### User Inputs:
- Dimensions (int)
- Liquid Type: Oil,Water & Fuel 
- Shift Priority (int)
- Current Level (%) (int)
- Thickness (int)
- Material type: Plastic, Steel & Aluminum (Enum)

#### Methods:
- `weight()`
- `volume()`
- `moment()`
- `unique_name()`
- `current_capacity()`


## Display tank statistics
## Store tank in cargo manifest
## Blender Settings
  ![Ensure your scale is set to 1](./change_units.png)
  
## Syntax
```py
import bpy
class MyProperties(bpy.types.PropertyGroup):
    
    my_string : bpy.props.StringProperty(name= "Enter Text")
    
    my_float_vector : bpy.props.FloatVectorProperty(name= "Scale", soft_min= 0, soft_max= 1000, default= (1,1,1))
    
    my_enum : bpy.props.EnumProperty(
        name= "Enumerator / Dropdown",
        description= "sample text",
        items= [('OP1', "Add Cube", ""),
                ('OP2', "Add Sphere", ""),
                ('OP3', "Add Suzanne", "")
        ]
    )

class ADDONNAME_PT_main_panel(bpy.types.Panel):
    bl_label = "Main Panel"
    bl_idname = "ADDONNAME_PT_main_panel"
    bl_space_type = 'VIEW_3D'
    bl_region_type = 'UI'
    bl_category = "New Tab"

    def draw(self, context):
        layout = self.layout
        scene = context.scene
        mytool = scene.my_tool
        
        layout.prop(mytool, "my_string")
        layout.prop(mytool, "my_float_vector")
        layout.prop(mytool, "my_enum")

        layout.operator("addonname.myop_operator")


class ADDONNAME_OT_my_op(bpy.types.Operator):
    bl_label = "Add Object"
    bl_idname = "addonname.myop_operator"
    
    
    
    def execute(self, context):
        scene = context.scene
        mytool = scene.my_tool
        
        if mytool.my_enum == 'OP1':
            bpy.ops.mesh.primitive_cube_add()
            bpy.context.object.name = mytool.my_string
            bpy.context.object.scale[0] = mytool.my_float_vector[0]
            bpy.context.object.scale[1] = mytool.my_float_vector[1]
            bpy.context.object.scale[2] = mytool.my_float_vector[2]


            
            
        if mytool.my_enum == 'OP2':
            bpy.ops.mesh.primitive_uv_sphere_add()
            bpy.context.object.name = mytool.my_string
            bpy.context.object.scale[0] = mytool.my_float_vector[0]
            bpy.context.object.scale[1] = mytool.my_float_vector[1]
            bpy.context.object.scale[2] = mytool.my_float_vector[2]
            
        
        
        
        if mytool.my_enum == 'OP3':
            bpy.ops.mesh.primitive_monkey_add()
            bpy.context.object.name = mytool.my_string
            bpy.context.object.scale[0] = mytool.my_float_vector[0]
            bpy.context.object.scale[1] = mytool.my_float_vector[1]
            bpy.context.object.scale[2] = mytool.my_float_vector[2]
            
        
        
        return {'FINISHED'}
    




classes = [MyProperties, ADDONNAME_PT_main_panel, ADDONNAME_OT_my_op]
 
 
 
def register():
    for cls in classes:
        bpy.utils.register_class(cls)
        
        bpy.types.Scene.my_tool = bpy.props.PointerProperty(type= MyProperties)
 
def unregister():
    for cls in classes:
        bpy.utils.unregister_class(cls)
        del bpy.types.Scene.my_tool
 
 
 
if __name__ == "__main__":
    register()
```
