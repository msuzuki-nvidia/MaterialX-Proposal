# MaterialX Hextiling Node Proposal


## Summary

Hextiling provides tiling a texture image without an obvious repeating pattern. This document proposes two new MaterialX hextiling nodes based on the article "Practical Real-Time Hex-Tiling" (Morten S. et al, 2022) \[1\].

* `<hextiledimage>`
* `<hextilednormalmap>`

The abstract of the article is as follows:

> To provide a convenient, easy-to-adopt approach to randomly tiled textures in the context of real-time graphics, we propose an adaptation of the by-example noise algorithm of Heitz and Neyret. The original method preserves contrast using a histogram-preserving method that requires a precomputation step to convert the source texture into a transform and inverse transform texture, which must both be sampled in the shader rather than the original source texture. Thus deep integration into the application is required for this to appear opaque to the author of the shader and material. In our adaptation we omit histogram preservation and replace it with a novel blending method that allows us to sample the original source texture. This omission is particularly sensible for a normal map as it represents the partial derivatives of a height map. In order to diffuse the transition between hex tiles, we introduce a simple metric to adjust the blending weights. For a texture of color, we reduce loss of contrast by applying a contrast function directly to the blending weights. Though our method works for color, we emphasize the use case of normal maps in our work because non-repetitive noise is ideal for mimicking surface detail by perturbing normals.

Unlike standard image tiling, hextiling involves image blending. As described in the abstract above, simple image blending is not suitable for normal maps; therefore, separate nodes for color images and normal maps are necessary.


## Parameters

### `<hextiledimage>`

| Parameter            |   Type   |  Range   |     Default     | Description                                   |
| -------------------- | :------: | :------: | :-------------: | --------------------------------------------- |
| **file**             | filename |          |                 | Image file path                               |
| **default**          |  color3  |          | (0.0, 0.0, 0.0) | Default color                                 |
| **texcoord**         | vector2  |          |      `UV0`      | Texture coordinate                            |
| **tiling**           | vector2  |          |   (1.0, 1.0)    | Tile repeat                                   |
| **rotation**         |  float   | \[0, 1\] |       1.0       | Rotation randomness                           |
| **rotation_range**   | vector2  |          |  (0.0, 360.0)   | Range of values used for randomizing rotation |
| **scale**            |  float   | \[0, 1\] |       1.0       | Scale randomness                              |
| **scale_range**      | vector2  |          |   (0.5, 2.0)    | Range of values used for randomizing scale    |
| **offset**           |  float   | \[0, 1\] |       1.0       | Offset randomness                             |
| **offset_range**     | vector2  |          |   (0.0, 1.0)    | Range of values used for randomizing offset   |
| **falloff**          |  float   | \[0, 1\] |       0.5       | Falloff width for blending tiles              |
| **falloff_contrast** |  float   | \[0, 1\] |       0.5       | Falloff contrast for blending tiles           |

### `<hextilednormalmap>`

| Parameter          |   Type   |  Range   |   Default    | Description                                   |
| ------------------ | :------: | :------: | :----------: | --------------------------------------------- |
| **file**           | filename |          |              | Image file path                               |
| **default**        | vector3  |          |   `Nworld`   | Default normal                                |
| **texcoord**       | vector2  |          |    `UV0`     | Texture coordinate                            |
| **tangent**        | vector3  |          |   `Tworld`   | Tangent vector                                |
| **bitangent**      | vector3  |          |   `Bworld`   | Bitangent vector                              |
| **tiling**         | vector2  |          |  (1.0, 1.0)  | Tile repeat                                   |
| **rotation**       |  float   | \[0, 1\] |     1.0      | Rotation randomness                           |
| **rotation_range** | vector2  |          | (0.0, 360.0) | Range of values used for randomizing rotation |
| **scale**          |  float   | \[0, 1\] |     1.0      | Scale randomness                              |
| **scale_range**    | vector2  |          |  (0.5, 2.0)  | Range of values used for randomizing scale    |
| **offset**         |  float   | \[0, 1\] |     1.0      | Offset randomness                             |
| **offset_range**   | vector2  |          |  (0.0, 1.0)  | Range of values used for randomizing offset   |
| **falloff**        |  float   | \[0, 1\] |     0.5      | Falloff width for blending tiles              |
| **strength**       |  float   | \[0, 1\] |     1.0      | Normal map strength                           |


## Examples

### `<hextiledimage>`

<center>

| ![](./images/image_rotation.gif) | ![](./images/image_scale.gif)  | ![](./images/image_offset.gif) |
| :------------------------------: | :----------------------------: | :----------------------------: |
|      **Rotation (0 - 360)**      |     **Scale (0.5 - 2.0)**      |       **Offset (0 - 1)**       |
|   ![](./images/image_rso.gif)    | ![](./images/image_tiling.gif) |                                |
|  **Rotation + Scale + Offset**   |      **Tiling (1 - 100)**      |                                |

</center>

### `<hextilednormalmap>`

<center>

| ![](./images/normal_rotation.gif) | ![](./images/normal_scale.gif)  | ![](./images/normal_offset.gif) |
| :-------------------------------: | :-----------------------------: | :-----------------------------: |
|      **Rotation (0 - 360)**       |      **Scale (0.5 - 2.0)**      |       **Offset (0 - 1)**        |
|   ![](./images/normal_rso.gif)    | ![](./images/normal_tiling.gif) |                                 |
|   **Rotation + Scale + Offset**   |      **Tiling (1 - 100)**       |                                 |

</center>


## References

\[1\] Morten S. Mikkelsen, Practical Real-Time Hex-Tiling, *Journal of Computer Graphics Techniques (JCGT)*, vol. 11, no. 2, 77-94, 2022, [http://jcgt.org/published/0011/03/05/](http://jcgt.org/published/0011/03/05/)  
