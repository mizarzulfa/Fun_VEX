# FUN VEX Muscle

*A series of VEX code snippets, Learning process and demonstrations showcasing syntax and abilities within SideFX Houdini*
<p align="right"><small>Mizar Zulfa</small></p>

⚠️ **Please note that this repository is currently being developed.**

## DAY 1 - VEX / VOP

### From VOP to VEX

<details>
<summary>View contents</summary>

* [`VEX_TO_VOP`](#VEX_TO_VOP)



### VEX_TO_VOP

DAY 1 - Recreate VOP to VEX

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
<summary>Example</summary>

<img src="/Additional_images/VEX_VOP_INIT.png" width="2000px;"/>

[File](https://github.com/mizarzulfa/Fun_VEX/blob/main/File/VOP_and_VEX_Initial.hip) VOP_VEX_Init_DAY1

</details>

<details>
<summary>The equations used in these setups</summary>

<img src="/Additional_images/INIT_Graphing Sine Functions.gif" width="2000px;"/>

</details>