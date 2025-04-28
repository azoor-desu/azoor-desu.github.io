---
title: Rendering Basics
description: "Basics of rendering!"
date: 2024-09-11T14:06:21.439Z
preview: ""
tags: ["graphics", "cpp"]
categories: ["Programming"]
---

# Vertex Pipeline
The geometry/vertex pipeline is as follows:  
`Model >`  
`World >`  
`Camera >`  
`Lighting (if any) >`  
`Project (onto 2D cam Plane) >`  
`Clip >`  
`Persepctive division >`  
`NDC >`  
`Screen/Device Coords >`  
`Framebuffer`  
![](https://upload.wikimedia.org/wikipedia/commons/f/f8/Geometry_pipeline_en.svg)
## Model
The raw vertex points straight from a model file (.obj, .fbx etc).  
These points work off 0,0,0 as the center of the model.  
## World
AKA the **Model Transform**. Model > World. Exists for each unique object in the world.  
Applies the model's position, scale and rotation to "place" it into the world, with data from the Transform component.  
Concatenate the Scale, Rotation and Transform matrices together to get this matrix (in the order TRS).  
## View (Camera)
AKA the **View Transform**. World > Camera. Exists for each unique camera.  
Brings the objects in the world to the camera's local coordinate system.  

We just want to rotate all objects in the world and shift them relative to where the camera is in world space, so that the camera is now at 0,0,0. Simple, just find the e1' e2' e3' vectors. No need to calculate rotation.  

Data required: Camera position, look direction, global up direction.  

1. Determine if your camera should use a LEFT-HANDED or RIGHT-HANDED coordinate system.
   OpenGL and friends use a RIGHT-HANDED system, while DirectX uses LEFT-HANDED.
   Visualisation: [https://www.youtube.com/watch?v=TGbMzoJqV7c](https://www.youtube.com/watch?v=TGbMzoJqV7c "https://www.youtube.com/watch?v=TGbMzoJqV7c")
   ![[Pasted image 20240819232059.png]]
   The difference between a left/right hand system is if the positive Z-axis comes at your camera or goes away from the camera. The X and Y axis directions should remain the same for both systems.
2. Figure out e3' first (Z/blue axis). For RIGHT-HANDED system, take camPos - targetPos (vector towards camera). For LEFT-HANDED system, the other way (vector away from camera). Normalize this vector, you got e3'.
3. Next, figure out e1'. e2' (the camera y axis), e3' and global up and  all lie on the same plane. This means you can cross e3' and the global up to find e1' (X/red axis). Assuming global up is a unit vector, this cross product should result in a unit vector for e1', no normalization needed.
   >Note: Be aware of the order of cross product! For LEFT-HANDED system, do e3' X globalUp. For RIGHT-HANDED system, do globalUP X e3'.
4. Lastly, you can find e2' by crossing e1' and e3' together. Again, be wary about the cross product order.
   > For LEFT-HANDED system, do e1' X e3'. For RIGHT-HANDED system, do e3' X e1'.

Finally, throw e1' e2' e3' into a matrix.
```
| e1'.x e2'.x e3'.x camPos.x |
| e1'.y e2'.y e3'.y camPos.y |
| e1'.z e2'.z e3'.z camPos.z |
|  0     0     0       1     |
```
But hold on, this matrix is for LOCAL to GLOBAL. **ALL default e1'e2'e3' matrices are LOCAL TO GLOBAL.** So, we need to inverse that matrix! Inverting 3x3 is a pain... but thank goodness it's easy in this case. There's a special property of orthonormal 3x3 matrices (all 3 columns are orthogonal and normalized): the inverse of this matrix is the same as the transpose of this matrix! So we transpose the above matrix (the 3x3 portion) to get the below:
```
| e1'.x  e1'.y  e1'.z  -e1'·camPos |
| e2'.x  e2'.y  e2'.z  -e2'·camPos |
| e3'.x  e3'.y  e3'.z  -e3'·camPos |
|   0      0      0         1      |

Note: 4th col is a dot product of 2 vectors, and not a regular period.
```
https://stackoverflow.com/questions/2624422/efficient-4x4-matrix-inverse-affine-transform  
Potential way to speed up Ax=b operations without finding A inverse: https://www.johndcook.com/blog/2010/01/19/dont-invert-that-matrix/
## Lighting
Do it at this stage if you have it. It will be covered in later sections below.  
## Projection
2 Types of projection: Orthographic and Perspective.  

Orthographic Projection (aka Parallel projection): projection directions for all points are PARALLEL. Meaning the "clipping box" is a cuboid, not a trapezoid. 

Perspective projection: projection directions for all points are NOT PARALLEL. The "clipping box" trapezoid. All pixels from the start to end get scaled down accordingly to fit on the end plane. 'Gives the "fish eye" kind of view.  
### Orthographic Projection
Re-maps points in a volume in view space and plonks them into a 2x2x2 NDC space. Your cuboid volume in view space must be aligned to the axes, and can't be in an arbitrary position or shape.
6 values to define the bounding volume in view space are specified to define this volume. (left, right, bottom, top, near, far)  
![alt text](/assets/img/blog/rendering/orth-proj.webp)  
A few things need to happen:
1. Transform (move) the volume to be centered at origin
2. Scale the points in the x, y, and z axes to fit into the 2x2x2 NDC space.
#### Transform
Find the center of your arbitrary volume using the 6 values, and then apply a transform. Simple.
```
| 1 0 0 -Px |
| 0 1 0 -Py |
| 0 0 1 -Pz |
| 0 0 0  1  |
```
#### Scale
Find the size of your bounding volume, normalize the points into a 1x1x1 box, then x2 to the values to fit into a 2x2x2 volume.
Working on a singular axis for visualization:
```
Let point be x
Let box width be w

Map component x from a w*h*d box into a 2*2*2 box:
x' = x / w * 2

Matrix to represent this mapping:
| 2/w  0   0  0 |   | x |
|  0  2/h  0  0 |   | y |
|  0   0  2/d 0 | * | z |
|  0   0   0  1 |   | 1 |
```
Of course, replace Px etc and w/h/d with the 6 corner values if needed.

Concat the 2 matrices together, you get this:
```
| 2/w  0   0  -Px |   | x |
|  0  2/h  0  -Py |   | y |
|  0   0  2/d -Pz | * | z |
|  0   0   0   1  |   | 1 |
```
One last caveat: OpenGL does its Z axis in the negative direction, so flip the Z value in the matrix to ensure your Z value in the NDC remains positive.
```
| 2/w  0   0   -Px |    | x |
|  0  2/h  0   -Py |    | y |
|  0   0  -2/d -Pz | *  | z |
|  0   0   0    1  |    | 1 |
```
Substituting w,h,d with the 6 values that represent the frustum (left, right, bottom, top, near, far), we get a matrix like so:  
![mtx](/assets/img/blog/rendering/orth-mtx.webp)  

### Simple Perspective Projection
![Youtube Explanation](https://www.youtube.com/watch?v=U0_ONQQ5ZNM)  
Use the depth (Z) to calculate how much to map the X and Y values. Assume the Center of Projection is at origin.  

Define your projection plane (aka near plane) using a Z value, for example, -5. Denoted as Nz. Have a point, let its Z value be -6. Denoted as Pz.  
![mtx](/assets/img/blog/rendering/orth-mtx.webp)      
To project the point P onto the projection plane, Pz' should be equal to Nz (-5). However, the X and Y values should be "interpolated". The value to interpolate by is the ratio of Nz to Pz.

To find `Px'` and `Py'`, do: 
`Px' = Px / Pz * Nz`

Apply this formula to `Px, Py` and `Pz` to get `Px', Py'` and `Pz'`. 

Rewriting this to become easier to work with will give: 
`Px' = Px * Nz / Pz`

`Pz'` SHOULD end up equal to `Nz`.

>Note: If the sign of both `Pz` and `Nz` values are different, it means the projection plane is on the wrong side, or your point is behind the camera. Either cull the point, or the point will be rendered upside down and the player can see behind the camera.

To express this in a matrix WITH perspective division, write it as such:
```
| Px * Nz |   | Nz  0  0  0 |   | Px |
| Py * Nz |   |  0 Nz  0  0 |   | Py |
| Pz * Nz | = |  0  0 Nz  0 | * | Pz |
|   Pz    |   |  0  0  1  0 |   | 1  |
```
With the resultant matrix, divide the x y and z values with the w whenever convenient to get the final `Px' Py' Pz'` values.

Again, note that both your `Pz` and `Nz` values are negative if the camera is pointing towards the -Z axis. Having the final divisor w be a negative will fix the x, y being negative and make z negative. Replacing the "1" with a "-1" in the matrix will do just that.

From here, you can map the values to NDC by slapping it through a Screen Space to NDC matrix or something (now working in 2D space), or try to expand on this idea more by doing a full perspective projection.
### Full Perspective Projection
Now, we want to "preserve" the `Pz'` value after our projection.
Reviewing what we have previously:
```
| Nz  0  0  0 |   | Px |   | Px * Nz |   | Px * Nz / Pz |
|  0 Nz  0  0 |   | Py |   | Py * Nz |   | Px * Nz / Pz |
|  0  0 Nz  0 | * | Pz | = | Pz * Nz | = | Pz * Nz / Pz |
|  0  0  1  0 |   | 1  |   |   Pz    |
```
Focusing on the final Z value: `Pz * Nz / Pz`
We can see that the z value of the evaluated point always resolves to Nz at the end. That's not good.

We'd want our final Z value to match the original `Pz` value of the original point in the ideal scenario. This means that we can mostly ignore `Nz` for now and try to obtain `Pz` as our final Z value.
This means that, for the previous step, instead of obtaining `Pz * Nz`, we should ideally obtain `Pz`^2
```
| Nz  0  0  0 |   | Px |   | Px * Nz |   | Px * Nz / Pz |
|  0 Nz  0  0 |   | Py |   | Py * Nz |   | Px * Nz / Pz |
|  ?  ?  ?  ? | * | Pz | = |   Pz^2  | = | Pz * Pz / Pz |
|  0  0  1  0 |   | 1  |   |   Pz    |
```
The only 2 values in the matrix we have to work with are the Nz value and the 0 to its right. Let's label them m1 and m2.
```
| Nz  0  0  0  |   | Px |
|  0 Nz  0  0  |   | Py |
|  0  0 m1  m2 | * | Pz | = |   Pz^2  |
|  0  0  1  0  |   | 1  |
```
Focusing on the Z row, we can try to make an equation to obtain Pz^2 from this:
`m1*Pz + m2 = Pz^2`

Unfortunately, there isn't a straightforward solution. There is no way to obtain `Pz`^2 for all values of `Pz`.
This is now a quadratic equation, which results in a curve.

Solve for m1 and m2 simultaneously. This means we have to plug something into Pz. What values should we plug? Well, the near and far planes! (because what other values asre there to use lol)
When `Pz == n`, we get `m1*n + m2 = n^2`
When `Pz == f`, we get `m1*f + m2 = n^f`
Solve simultaneously to get: 
`m1 = f + n`
`m2 = -fn`

Plug this into the matrix:
```
| Nz  0  0   0  |   | Px |
|  0 Nz  0   0  |   | Py |
|  0  0 f+n -fn | * | Pz | = | Pz^2... sometimes |
|  0  0  1   0  |   | 1  |
```
This solution is not the most ideal, but it's the closest we can get to the ideal value Pz.
If we dig deeper, we can see that this final Z value has the property of `1/Pz`:
```
Focusing on Z row: [...matrix...] = f+n-fn(1/Pz) = final Z value
```
Problems arise when n and f are too far apart, and the values obtained are too close together, thus resulting in z fighting.
![](https://developer-blogs.nvidia.com/wp-content/uploads/2015/07/depth-perception-graph1-b.jpg)
More on Z-fighting due to precision loss: https://developer.nvidia.com/blog/visualizing-depth-precision/
To summarize:
- The closer your z values are to 0, the more bunched up these ticks on the Z-axis will get.
- Moving objects a tiny bit in this z range will cause your d (depth) value to jump much more than if you moved the same tiny bit further out towards the far plane, causing higher chance of Z-fighting
- Floating-point precision inconsistencies with slightly different camera angles can be enough to cause the tiny value changes on the z-axis
- Moving your near plane closer to 0 will cause more intense Z-fighting as more values reside on the steeper portions of the graph. Moving the near plane further out will reduce Z-fighting.

One way to combat this issue, Reverse Z: https://tomhultonharrop.com/mathematics/graphics/2023/08/06/reverse-z.html
![](https://developer-blogs.nvidia.com/wp-content/uploads/2021/05/depth-precision-graph5-625x324.jpg)
We map the 0 and 1 the other way round instead, so that when Z-fighting happens, it happens further away from the camera. Now, moving your far plane further away will result in a higher chance of Z-fighting, leaving the closer objects less prone to Z-fighting.

Aite great! Full perspective projection done, put it together with mapping into NDC and that's it.

# Lighting calulations
## Shading
https://en.wikipedia.org/wiki/Shading
Shading is done by using a light source and a surface normal to determine the final color of what a pixel should be, based on the cosine of the angle of the normal and the light source.

At 0 degrees (cos0 = 1), the light intensity should be 1. 
At 90 degrees (cos90 = 0), the light intensity should be 0. 
At >90 degrees, clamp light intensity at 0 (because you can't have negative light).

Dot product of 2 unit vectors conveniently gives us the cosine value, so we can use the dot product.
![[Pasted image 20240904221918.png]]

### Faceted (Flat) Shading
Super-fast shader.
Basic idea is that all pixels on a triangle surface uses the same normal for lighting calculation. Since triangles have a flat surface, this means that all pixels in this triangle will have the same color value. The normal should be the triangle surface's normal.
![[Pasted image 20240904221948.png]]

End result will look very... triangle-y. Each triangle face will be visibly shaded, but it's super fast. 
![](https://help.autodesk.com/cloudhelp/2023/ENU/MotionBuilder/images/GUID-A9B5C225-B9B6-4959-AB74-E5BF7739F0AA.png)

### Gourad/Phong Shading
Gourad shading: 
The light intensity is calculated per vertex, and then this value is interpolated and passed to each fragment for color computation.
> Majority of computation is offloaded to vertex shaders, making it fast, but lighting can look bad on low-poly models.

Phong shading:
The normal is obtained per vertex, and this value is interpolated and passed to each fragment to calculate the light intensity per fragment, then applied to color computation.
> Lighting is mostly performed on each fragment, lowering performance but improving the light quality

![](https://i.pcmag.com/imagery/encyclopedia-terms/gouraud-shading-_shading.fit_lim.size_1050x.gif)

## Normal Mapping
https://learnopengl.com/Advanced-Lighting/Normal-Mapping
Surface normals are used for lighting calculations.
They are usually per-vertex or per-pixel. Per-pixel normal values can be stored in a "normal map"

Normals help to calculate the angle of reflection of a texture surface. For a flat plane, the entire plane will have the same normal, thus it would make sense that it should look "smooth". For a bumpy wall, it would be expected that this wall would have some minor "bumps", and light bouncing off of this surface should not look flat. We can induce this effect using per-pixel normals that point in slightly different directions to make the surface not feel as "flat".
![](https://learnopengl.com/img/advanced-lighting/normal_mapping_surfaces.png)
When applied to an actual surface, it look like this:
![](https://learnopengl.com/img/advanced-lighting/normal_mapping_compare.png)

Ususally, raw model files (.fbx, .obj etc) should have per-vertex normals included. Per-vertex normals can be used to obtain per-fragment normals (blinn-phong), and then that would be used for lighting. However, custom bumpy surfaces like brick walls can't be interpolated from model per-vertex normals (how are you going to generate the normals along the seams and crevices?), so artists usually provide a normal map to get the accurate per-pixel lighting they want. These normal maps can be generated by their art/modelling software.
![](https://artisticrender.com/wp-content/uploads/2022/05/ObjectNormalExample.jpg)
### Types of Normal Maps
There are a few flavours when it comes to normal maps.
You could have your normal maps in different systems: tangent space, object space and world space.
The difference is the direction that these normals are pointing in, or how they are interpreted.
![](https://lh5.googleusercontent.com/proxy/nvwe6QpZJ4YJLaW65I5llM1Tvk6DLz6pt2FBVqLtFD6DPYo8GUmCKSltBJ8eOrr2fe8vBplwRiW8sd2Tv_Gv86vzfAeOO31OdZKlvut2pcJ1XnoG9g)
#### Tangent Space
![](https://upload.wikimedia.org/wikipedia/commons/thumb/6/66/Image_Tangent-plane.svg/1920px-Image_Tangent-plane.svg.png)
Tangent space maps always have the Z-axis pointing up away from the surface of the object (assuming a flat surface). Thus, the normal map will always have a blue-ish tint consistently across the whole map (because Z corresponds to B in a vector3).
![](https://learnopengl.com/img/advanced-lighting/normal_mapping_normal_map.png)
#### Object Space
Each normal has their axes aligned to the axes of the object itself. This is different from tangent maps, where each normal's axes are aligned according to the surface of wherever each pixel rests on.
#### World Space
Similar to object space, but instead of the axes aligning to the object, axes are aligned to the world.
#### What should you use
From [this thread](https://polycount.com/discussion/227725/difference-between-object-space-and-world-space-normals):
>Tangent space maps are more reusable (tiling textures for instance), and also work with skinned/deforming objects.
They are also more prone to artefacts and the lighting calculation is slightly more complex (but negligible)
For a non deforming asset with its own normal map, you could use object space.
For completely static object with its own normal map, you could use world (but no one do this).
Pretty much everybody use tangent.
#### More reading
https://blenderartists.org/t/tangent-vs-object-space-vs-world-space/456423
## Lighting calculations with Tangent Space
Normally, if we'd used world space or object space normals, computing the angle of reflection of a light ray is simple enough, as everything is in (almost) the same coordinate system.
Let's assume we are going with tangent space normals instead.
In order to use tangent normals for lighting, we'd need the light source and the normal to be in the same coordinate system. Right now, they are in different systems, world space and tangent space. Thus, we'd need to build a change-of-basis matrix to convert either the light or normal to the other space. For that, we'd need to obtain the 3 basis vectors to build the change-of-basis matrix, which are the Normal, Tangent and Bi-Tangent vectors. We'll need these vectors on a per-vertex basis.
### Calculating Tangent and Bi-Tangent
https://learnopengl.com/Advanced-Lighting/Normal-Mapping
Skippa skippa. Just note that this change of base matrix is called a TBN (Tangent, BI-tangent, Normal) matrix.
TLDR: Calculate it manually when importing your assets, or use the importer tool (e.g. assimp) to generate tangent and bi-tangents.
> If you're generating a custom mesh (e.g. generating terrain) you might have to do this calculation manually. This is usually implemented in tooling and not really for game runtime so that's a problem for next time.

After obtaining thsoe vectors, these per-vertex vectors should look something like that, witht the blue z axes pointing away from the surface instead of globally up:
![](https://learnopengl.com/img/advanced-lighting/normal_mapping_tbn_shown.png)
### Applying TBN in shaders
Our light and normals are still in different coordinate systems. But we now have a change-of-basis matrix!
There are 2 ways to go about computing lighting:
1. Convert everything into world space and compute in world space
2. Convert everything into tangent space and compute in tangent space

Hint: Computing in Tangent space is more efficient. Read more below.
#### 1. Computing in World Space
Need to convert the tangent space normal into world space normal. Light direction is already in world space.
Compute the TBN matrix per-vertex, then pass the info to the fragment shader. It should look something like this in glsl vertex shader:
```
// Inputs from host
layout (location = 0) in vec3 aPos;
layout (location = 1) in vec3 aNormal;
layout (location = 2) in vec2 aTexCoords;
layout (location = 3) in vec3 aTangent;
layout (location = 4) in vec3 aBitangent;  

// outputs for later shader stages (frag shader)
out VS_OUT {
    vec3 FragPos;
    vec2 TexCoords;
    mat3 TBN;
} vs_out;

void main()
{
   [...]
   vec3 T = normalize(vec3(model * vec4(aTangent,   0.0)));
   vec3 B = normalize(vec3(model * vec4(aBitangent, 0.0)));
   vec3 N = normalize(vec3(model * vec4(aNormal,    0.0)));
   vs_out.TBN = mat3(T, B, N); // compute TBN matrix and pass to frag shader
}
```
In the fragment shader, you'd want to grab the normal from the texture map (still in tangent space), and then apply this TBN matrix to the tangent space normal. You'll get a world space normal. Now you can use this world space normal for lighting calculations!
```
// Input from vert shader (WARNING: Values are interpolated)
in VS_OUT {
    vec3 FragPos;
    vec2 TexCoords;
    mat3 TBN;
} fs_in;  

normal = texture(normalMap, fs_in.TexCoords).rgb;   // extract normals from normal map. Normal is in Tangent Space.
normal = normal * 2.0 - 1.0;                        // Re-adjust normals extracted from map to turn it from range [0, 1] to [-1, 1]
normal = normalize(fs_in.TBN * normal);             // Apply TBN matrix to tangent space normal, get world space normal.
```
> Note: When transferring data from vertex to fragment shader, values can't be passed 1 to 1. Instead, values passed from vertex shaders to fragment shaders are interpolated depending on their position in the triangle. This also means the values in your matrix are interpolated as well. This may cause the matrix to end up not being orthogonal when it reaches your fragment shader. You can try to re-orthogonalize the matrix by extracting 2 of the axes and then re-calculating the 3rd axis from cross-product-ing. Or you could use the matrix as-is but risk inaccurate normals.

#### 2. Computing in Tangent Space
In the vertex shader, we compute the TBN matrix as usual. But this time, we need an inverse matrix of TBN, as we're converting things the other way round. Due to the matrix being orthogonal, transposing the matrix is the same as inverting the matrix.

Now, instead of passing in the matrix to the fragment, we perform matrix multiplication in the vertex instead. What do we multiply? _Not_ the normal, because that's in the fragment and not vertex. We instead pre-compute all the values we need for lighting calculations first before sending it over to the fragment shader. 
This means that you should pre-compute:
- TangentLightPos
- TangentViewPos
- TangentFragPos

With these 3 values, and the tangent normal later, we can compute the lighting.
```
// VERTEX SHADER
out VS_OUT {
    vec3 FragPos;
    vec2 TexCoords;
    vec3 TangentLightPos;
    vec3 TangentViewPos;
    vec3 TangentFragPos;
} vs_out;

uniform vec3 lightPos;
uniform vec3 viewPos;
 
[...]
  
void main()
{    
    [...]
    mat3 TBN = transpose(mat3(T, B, N));
    vs_out.TangentLightPos = TBN * lightPos;
    vs_out.TangentViewPos  = TBN * viewPos;
    vs_out.TangentFragPos  = TBN * vec3(model * vec4(aPos, 1.0));
}  

// FRAG SHADER
void main()
{           
    vec3 normal = texture(normalMap, fs_in.TexCoords).rgb;
    normal = normalize(normal * 2.0 - 1.0);   
   
   // normalize values because interpolated values from fs_in may not be normalized anymore
    vec3 lightDir = fs_in.TBN * normalize(lightPos - fs_in.FragPos); 
    vec3 viewDir  = fs_in.TBN * normalize(viewPos - fs_in.FragPos);    
    [...]
}  
```
