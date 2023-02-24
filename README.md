# FUN VEX Muscle

[![Github](https://img.shields.io/badge/-Github-000?style=flat&logo=Github&logoColor=white)](https://github.com/mizarzulfa)
[![Linkedin](https://img.shields.io/badge/-LinkedIn-blue?style=flat&logo=Linkedin&logoColor=white)](https://www.linkedin.com/in/mizarzulfa/)
[![Instagram](https://img.shields.io/badge/-Instagram-c13584?style=flat&labelColor=c13584&logo=instagram&logoColor=white)](https://www.instagram.com/mizarzulfa/)
[![Gmail](https://img.shields.io/badge/-Gmail-c14438?style=flat&logo=Gmail&logoColor=white)](mailto:contact@mizarzulfa.com)

&nbsp;

*A series of VEX code snippets, Learning process and demonstrations showcasing syntax and abilities within SideFX Houdini*
<p align="right"><small>Mizar Zulfa</small></p>

⚠️ **Please note that this repository is currently being developed.**


# Topics
* [VOP/VEX - Loop - Sin()](##VOP_TO_VEX)
* [Animated Noise with Cross Product](##aanoise)
  

<!-- LIST 1  -->
### From VOP to VEX

<details>
<summary>View contents</summary>

* [`VOP_TO_VEX`](##VOP_TO_VEX)

DAY 1 - Recreate VOP to VEX (LOOP - Object Hovering)

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
### Animated Noise with Cross Product

<details>
<summary>View contents</summary>

* [`aanoise`](##aanoise)

DAY 1 - Recreate VOP to VEX (LOOP - Object Hovering)

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
