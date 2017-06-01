# Project 2 - Journal
(**S**' point of view)

## 04.19

I and **L** are planning to build our very own model for the project. We still don't know the subject, but we're going to figure out these days while I catch up with the lessons.

**L** has a fairly good understanding of modeling and uv mapping, and of web programming too, so we decided he'll build the model and write the menu, I'll probably take care of the shaders and materials.

## 04.21

I was thinking about the modeling process, asking myself if I could do it myself and how, and outlined a few problematic tasks I have little to no idea how to handle because of my lack of experience, and pointed them out to **L** for him to be aware of.
Specifically, I can't seem to be able to wrap my head around the texturing process. How does one put a texture on a model: does one project a texture on the model or even paint it directly in 3D space, or map it to the UV space and paint in 2D space instead, being careful to hide the seams somehow?
Second, is it better to start from a sketchy low-poly version of the model, or "sculpt" it at high difinition and re-topologize it at a later stage?

And so on. There are so many variables and they're orthogonal to each other, so the possibilities are practically infinite. Our best option is a hard-surfaced model, such as a robot or a military vehicle.

## 04.24

Since we still don't know what to model I can't really work on anything, so in the meantime I've been watching some good tutorials I've found online about modeling and texturing. I'm learning, painfully slowly.

## 04.25

**L** has written a basic version of the menu. The idea is that some elements will be moving on the canvas to match the model, e.g. to point out some changes occurring on it, thus focusing the attention of the end user.
We're thinking of divvy up the workload of the modeling.

## 05.03

**L** had proposed a drone as the subject of the modeling, which I happened to like, but we've been wasting time lately looking at real-world as well as fictional references and drawing a few sketches of what it should look like, the latter not satisfying neither of us.
**L**'s first attempt at modeling is remarkable but the topology looks like will be a pain to work with later down the pipeline, and we can't seem to find a decent free-to-use model online, so in the end we decided I'll begin modeling the drone instead, following one last raw sketch I did, while he'll keep working on the menu, which seems reasonable as the menu actually is an important requisite of the project and should need more of his attention, in my opinion.

## 05.05

As one can see, I decided to model the drone, starting with its low-poly version, by pieces, always beginning with cubes or cylinders or spheres as basic shapes, so that they are going to be fairly easy to unwrap and also this method is simple and well suited for hard-surfaced models.

## 05.07

The low-poly version of the model is nearly done, I'm prepping for the unwrap phase. The fact that I still don't know how to deal with texture phase in an efficient way is bothering me.

## 05.09

I finished unwrapping the model.

There are some minor adjustments to make, but we're almost there. Since we're left with just over a week of time we decided it's best we don't switch tasks and _I_ do the details of the drone instead, which in fact will basically consist of a rounding of the hard edges. **L** is working on the environment cubemap.
In the end I resolved to email the professor as to which method is the best for the texturing phase. I think I'm just going to paint on the 2D space, which I think is more straightforward, at the cost of a less professional-looking result.

## 05.10

I'm having trouble trying to pack the uv islands of all the single pieces into a common UV space, I think I'm approaching a deadlock and I'm losing precious time. I can't join the geometries because I need them separate if I want to be able to animate them...assuming the `OBJ` loader is able to preserve grouping information, which I don't know of.

-

I think I've found a neat solution: it looks like `Blender` has a plug-ing named `Texture Atlas`, which helps keeping track of the UV islands as well as define more than one UV layer, speaking of which I think I'm going to separate the model in two parts, one of them being the most prominent/detailed so it's certainly going to need more texture space.

## 05.13

I've had a very hard time setting up the UVs. I went throught all the island packing thing three more times before managing to get their relative size and angle stretch factor, the efficiency of texture space utilization and the double UV layer under control.
As predicted the detailing phase didn't take me long, but we're still way behind and I'm beginning to think we won't make it in time for the deadline.

I'm just now beginning to work on the normal and ambient occlusion maps, which I'm going to bake directly in `Blender` as demostrated in one of the tutorials I downloaded.

## 05.15

Normal maps turned out pretty good, but I had to switch to smooth shading to average the normals and go again through all the pieces of the model for setting the so-called sharp edges, which should help define the shape of the objects where the curvature angle gets very narrow, so in fact they almost coincide with the UV seams. Almost.
Some details of the normal maps were obtained by "projecting" a separate hi-res geometry onto the low-res one via ray casting, in a separate baking session.

By the way I had to unwrap and repack the whole model one more time, because I found out some pieces happened to exhibit double or hidden vertices, edges and faces, and n-gons (which were quite dsiturbing to find out in the most improbable of the places), not to mention that the latter of which are not even supported by `Three.js`' `OBJ` loader. Good thing I haven't started texturing yet.

**L** is searching something on lights, how to light up the product like in advertising and catalogues.

## 05.16

I'm still trying to bake the ambient occlusion maps, which are harder to get than I thought because some object need not affect some others that are going to be able to move, as they would exhibit the occlusion properties of their original relative orientation, which would be wrong.

We're running out of time.

## 05.17

Our professor told us he decided to shift the deadline for everyone to the end of the month, which should be more than enough.

## 05.22

I managed to obtain some good-looking ambient occlusion maps and some basic diffuse, specular and roughness maps, thanks to `Blender`'s PBR support.

Nonetheless I've been having some troubles with my laptop. `Blender` keeps crashing for no reason other than a lack of memory, I suppose, and I think my hard drive has some faulty sectors I'm trying to recover right now.

## 05.24

I noticed the normal maps still present some artifacts, which for the life of me I can't figure how to get rid of. I dowloaded and used `xNormal` for re-baking a few pieces individually, which helped but the problem would persist.
Also the two helices of the drone are mirrored so they utilize the same texture space, so I had to tell the shader to invert the tangent and binormal vectors for the mirrored copy.

On the other hand the colour maps are turning out good; the UV seams are getting in the way as predicted, but honestly I don't have much time to pay attention to them, and also I'm trying to keep the paint confined to a single UV island every time, so the problem doesn't occurr as much as it would otherwise and the results are still pretty decent.

Also, I found a neat `Blender` project online that is conveniently set up to output the render of its scene into a cubemap, which allowed me to convert the latlong map of one of the examples we saw during the lessons into its cubemap form, and even compute its mipmap levels up to the smallest one to use for simulating the reflections, without encountering the problem of the seams at the edges of the cube.
The very same cubemap-transformed environment map was used to feed ATI's (`Modified` by S. Lagarde) `CubeMapGen` for calculating the corresponding irradiance map using the spherical harmonics method (which is admittedly dark in our case but still makes a difference). I'm considering bumping up its saturation a bit, for artistic purposes.

## 05.26

The maps are pretty much done, but I feel like they still need some detailing. Also I've been working on the animations of the drone and the camera, and I'm about to begin refining the menu and bind it to the `javascirpt` code. Specifically, the animations for the eye are made with the method of quaternion interpolation.

## 05.27

Writing even a simple `html/css` menu is harder than I thought--honestly much harder than it should. I hope **L** can fix it up within the day after tomorrow at latest. He's still working on the light setting. At least I managed to make it work and interface with the main script code.

## 05.28

I've worked a bit more on the animations, which are now more fluid/realistic, even if not by much. Also I had fun writing a little shader for the eye of the drone, for which I used `ShaderFrog` to test as suggested during the course.
Tomorrow will be devoted to one last diffuse map variant, which shouldn't take more than a few hours to build, a spotlight for the drone's torch and some other minor refinements, given I'll have the time, otherwise the project looks pretty much complete anyways and the requirements are met.

Actually I think I'll be working on the details of the roughness and specular maps first, and then create a third diffuse map later.

## 05.29

I've adjusted/corrected shader, textures and lights; **L** will fix the menu. Eventually I opted to neglect the third variant of the diffuse map.

## 05.30

Our professor announced the deadline has been posponed by another week, so we're taking some time to polish the project and see if we can implement some of the other ideas we had in mind.
I happened to figure out the cause of the artifacts of the normal map: the problem was that there were too few polygons that were hosting the details of a smoothly curved surface, which caused xNormal to bake incorrectly. I added a few polygons in that area and baked the normal maps again, and now they're behaving correctly.

By testing the shader components separately it looks like the function `BRDF_Specular_GGX_Environment` already embodies the Fresnel term, which on the other hand is obviously absent in the computation of the irradiance component--which should complement the reflectance one, so in the end I added such term manually.

## 05.31

I've just fixed a few things.

## 05.32
**L:** I've added max and min zoom for the camera.

## 05.33
We eventually decided we'll leave the project as it is and concentrate on the final exam of the course instead.
