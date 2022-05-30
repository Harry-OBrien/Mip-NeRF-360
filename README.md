# mip-NeRF 360
## NeRF
**[NeRF paper](https://arxiv.org/pdf/2003.08934.pdf)**
* encodes the volumetric density and colour of a scene within the weights of a coordinate-based MLP (fully connected, no conv layers)
* trace rays through each input image
* sample points along each ray
* feed these points to a NN that predicts a colour and density for each sample
* perform alpha compositing along the ray to render a pixel

## mip-Nerf
**[Mip-NeRF paper](https://arxiv.org/pdf/2103.13415.pdf)**
* addresses aliasing and scale from the NeRF paper
* NeRF is a single scale model that cannot work on multi-scale problem

* Instead of ray casting, a cone is cast into the scene for each pixel
* radius of cone is determined by the size of the pixel on the image plane
* cone models whole volume space visible by that pixel
* renders average of whatever intersects with the volume (instead of the infinitely narrow ray) 

* instead of points, we now sample conical frustrums in the scene
* because conical frustrums are difficult to manipulte analytically, a multivaried gaussian is fit to the frustrum - a closed form solution
* THEN instead of positional encoding a single coordinate along the ray, we compute the expected positional encoding with respect to the Gaussian that was constructed
* this also has a simple closed form
* this is called an 'integrated closed form encoding'

## mip-NeRF 360
**[Mip-NeRF 360 Paper](https://arxiv.org/pdf/2111.12077.pdf)**
* Mip-NeRF doesn't work well on unbounded scenes

### Aspects building upon mip-NeRF
Mip-Nerf 360 does 3 key things:
Representation/Paramaterisation
* Applies a Kalman-like warm to mip-NeRF Gaussian into a non-euclid space

Efficiency
* Training is sped up 3x by 'distilling' a scene geometry from a large NeRF MLP into a small MLP during training

Ambiguity
* A novel regulariser is applied to reduce ambiguity of a large observation space, specifically for mip-NeRF ray intervals

