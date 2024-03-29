# FUN VEX Muscle

[![Github](https://img.shields.io/badge/-Github-000?style=flat&logo=Github&logoColor=white)](https://github.com/mizarzulfa)
[![Linkedin](https://img.shields.io/badge/-LinkedIn-blue?style=flat&logo=Linkedin&logoColor=white)](https://www.linkedin.com/in/mizarzulfa/)
[![Instagram](https://img.shields.io/badge/-Instagram-c13584?style=flat&labelColor=c13584&logo=instagram&logoColor=white)](https://www.instagram.com/mizarzulfa/)



*A series of VEX code snippets, Learning process and demonstrations showcasing syntax and abilities within SideFX Houdini*
<p align="right"><small>Mizar Zulfa</small></p>

⚠️ **Please note that this repository is currently being developed.**

## What is VEX?

VEX is a shading language used in Houdini that is similar to the Renderman Shading Language (RSL). It is a software interpreted language, which means that it provides the flexibility of scripting without requiring pre-compilation of code. Additionally, it has an implicitly SIMD evaluation approach, where your code is executed over multiple data such as vertices, points, primitives, pixels, and voxels.

VEX is used for a wide range of tasks, including geometry, volume and simulation processing, as well as shading and compositing. It is highly optimized for use in Houdini and provides a significant performance boost over other scripting or programming languages.





# Topics
  * **[From VOP to VEX](#from-vop-to-vex)**
  * **[VEX - vop\_fbmNoiseFP() for Anti-Aliased Noise](#vex---vop_fbmnoisefp-for-anti-aliased-noise)**
  * **[Animated Noise with Cross Product](#animated-noise-with-cross-product)**

<!-- LIST 1  -->
### From VOP to VEX

<details>
<summary>View contents</summary>

* DAY 1 - Recreate VOP to VEX (LOOP - Object Hovering)

```c
// Get values from user-defined channels
float frequency = chf('frequency');
float h = chf('Horizontal_shift');
float v = chf('Vertical_shift');
float a = chf('Amplitudo');

// Compute the initial value of the sine wave based on time and horizontal shift
float initial = 2 * $PI * frequency * @Time + h;

// Compute the value of the sine wave at the current time and add the vertical shift
float wave = a * sin(initial);
wave += v;

// Set the y coordinate of the point position to the value of the sine wave
@P.y = wave;
```

<details>
<summary>The equations used in these setups</summary>
<img src="/Additional_images/INIT_Graphing Sine Functions.gif" width="2000px;"/>
</details>

<br>[⬆ Back to top](#Topics)
</details>

<!-- ----------------------  -->


<br>

<!-- LIST 2  -->
### VEX - vop_fbmNoiseFP() for Anti-Aliased Noise

<details>
<summary>View contents</summary>

* VEX - vop_fbmNoiseFP() for Anti-Aliased Noise

```c
/////
#include <voplib.h>
vector4 pos = set(v@P.x, v@P.y, v@P.z, 0);
v@Cd = vop_fbmNoiseFP(pos, 0.5, 8, 'noise') + 0.5;
/////

float vop_fbmNoiseFF(float pos; float rough; int maxoctaves; string noisetype)
float vop_fbmNoiseFV(vector pos; float rough; int maxoctaves; string noisetype)
float vop_fbmNoiseFP(vector4 pos; float rough; int maxoctaves; string noisetype)
vector vop_fbmNoiseVF(float pos; float rough; int maxoctaves; string noisetype)
vector vop_fbmNoiseVV(vector pos; float rough; int maxoctaves; string noisetype)
vector vop_fbmNoiseVP(vector4 pos; float rough; int maxoctaves; string noisetype)

```

<details>
<summary>Description</summary>
 <!-- <img src="/Additional_images/INIT_Graphing Sine Functions.gif" width="2000px;"/> -->
  In Houdini, you can create anti-aliased noise with the vop_fbmNoiseFP() function from the voplib.h library.
  <br> The code includes this library using the #include directive in the first line.

  > To use the function, you need to specify the position vector, roughness, maximum octaves, and noise type. The function returns a value that you can add to a constant to color the point.

  > There are several vop_fbmNoiseXX() functions available, each with different return types and dimensions. You can use float (F), vector (V), or vector4 (P) depending on your needs.

> When calling the function, the first letter in the last two upper case letters indicates the return type: float (F) or vector (V). The second letter specifies the dimension of the position argument: float (F), vector (V), or vector4 (P).

Remember to include the library to avoid errors, and adjust the parameters to create different noise patterns in your project.
</details>

<br>[⬆ Back to top](#Topics)
</details>

<!-- ----------------------  -->

<br>

<!-- LIST 3  -->
### Animated Noise with Cross Product

<details>
<summary>View contents</summary>

* Animated Noise with Cross Product

```c
#include <voplib.h>   
    
// Calculate vector from current point to neighboring point with @ptnum 1
vector substract_vel = point(1, 'P', @ptnum) - @P;

// Cross product with the fixed vector {0,0,1}
vector crossnew = {0,0,1};
crossnew = cross(crossnew, substract_vel);

// Convert to Vector4 with the 4th component as the current time
vector4 pos_noise = set(crossnew.x, crossnew.y, crossnew.z, @Time);

// Apply an offset to the noise
float power = pow(rand(i@id), 0);
power = clamp(power, 0, 4);
float offset = power + chf('offset');

// Set the amplitude of the noise
float amplitudo = chf('amplitudo');

// Calculate Aanoise using the fbmNoiseFP function in VOPs
vector aanoise1 = vop_fbmNoiseFP(pos_noise + offset, 1, 32, 'noise') * amplitudo;
aanoise1 += crossnew;

// Set the calculated noise vector as the normal and velocity vectors
v@N = aanoise1;
v@v = aanoise1;
```

<details>
<summary>The equations used in these setups</summary>
 <!-- <img src="/Additional_images/INIT_Graphing Sine Functions.gif" width="2000px;"/> -->
</details>

<br>[⬆ Back to top](#Topics)
</details>

<!-- ----------------------  -->
<br>

<!-- LIST 5  -->
### Quick Notes : Modulo Operator

<details>
<summary>View contents</summary>

* 2 % 5

```c
when we use the modulo operator, we are essentially asking the question
 "What is the remainder when we divide the first number (in this case, 2)
  by the second number (in this case, 5)?"

In this case, we can divide 2 by 5 once, with a remainder of 2.
Therefore, the result of 2 % 5 is 2.
```

<details>
<summary>Real life Example</summary>
 <!-- <img src="/Additional_images/INIT_Graphing Sine Functions.gif" width="2000px;"/> -->

  > When we divide 2 by 5, we cannot divide it evenly into 5 parts, so there will be some left over. That left over part is called the remainder. In this case, the remainder is 2. To make it simpler, imagine you have 2 apples and you want to divide them equally among 5 people. Since you cannot divide the apples equally into 5 parts, you will have some pieces left over. The number of leftover pieces is the remainder. In this case, you will have 2 apples and no extra pieces, so the remainder is 2.

  > **1 % 5** : If you have a group of 5 people and you want to divide a bag of candy evenly among them, you would have 5 portions. However, if you only have 1 piece of candy, it cannot be divided evenly among the 5 people, and the remainder would be 1. Therefore, 1 % 5 would result in a remainder of 1.

</details>

<br>[⬆ Back to top](#Topics)
</details>

<!-- ----------------------  -->

<br>

<!-- LIST 6  -->
### Quick Notes : Creating Points and Applying a Normalized Polyline

<details>
<summary>View contents</summary>

* Creating Points and Applying a Normalized Polyline

```c
// Set the vector b to the value of the 'point' channel.
vector b = chv('point');

// If the 'normalize' channel is set to 1, normalize vector b.
if (chi('normalize'))
{
    b = normalize(b);
}

// Add a point to the geometry at the position of vector b and store the point index in p1.
int p1 = addpoint(0, b);

// Add a second point at the origin and store the point index in p2.
int p2 = addpoint(0, {0,0,0});

// Add a polyline primitive connecting points p1 and p2 to the geometry and store the primitive index in primm.
int primm = addprim(0, "polyline", p1, p2);

// Set the color attribute of points p1 and p2 to the absolute value of vector b.
// setpointattrib(0, "Cd", p1, abs(b), "set");
// setpointattrib(0, "Cd", p2, abs(b), "set");

```

<br>[⬆ Back to top](#Topics)
</details>

<!-- ----------------------  -->



