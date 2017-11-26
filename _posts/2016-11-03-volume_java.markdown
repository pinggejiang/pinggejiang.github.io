---
layout:     post
title:      "3D Volume object in Java"
subtitle:   ""
date:       2016-11-03 19:36:17
author:     "Pingge Jiang"
header-img: "img/home-bg.jpg"
catalog:    true
categories: research
tags:
    - [coding, java, volume]

---
This my implementation of build a volume for .mha file in java. Function "volume_create" creates the volume with specific parameters. Function "volume_cal_grad" calculates the gradient of a volume.

volume class:

volume proporties:

<br /> * volume dimension in 3D
<br /> * number of voxels
<br /> * volume offset
<br /> * voxel spacing in world coordinate
<br /> * voxel type stored in memory
<br /> * 2D image array for storing 3D voxels
<br /> * import java.util.Arrays;

## volume.java
```java
/*
* @File Volume.java
* @Author Pingge Jiang
* @Date November 3rd. 2016 
*/

public class Volume
{
/**
* @brief designed for .mha volume
* @param dim volume dimension
*	 npix total number of voxels in volume
*	 offset volume offset
*	 pix_spacing voxel spacing in world coordinates
*	 pix_type pixel type of image stored in memory
*	 img number of voxels array
**/
	public int[] dim = new int[3];
	public int npix;
	public float[] offset = new float[3];
	public float[] pix_spacing = new float[3];
	public String pix_type;
	public float[] img;
}
```

volume process class:

## sub functions:

<br /> * volume create
<br /> * volume_calc_grad

```java
public class Volume_proc
{
	public Volume volume_create(int[] dim, float[] offset, float[] pix_spacing,
        String pix_type, float[] direction_cosines, int min_size)
    	{
		Volume vol = new Volume();
		int i;
		for (i = 0; i < 3; i++)
		{
	    		vol.dim[i] = dim[i];
	    		vol.offset[i] = offset[i];
	    		vol.pix_spacing[i] = pix_spacing[i];
		}
		vol.npix = vol.dim[0] * vol.dim[1] * vol.dim[2];
		vol.pix_type = pix_type;
		switch (pix_type) 
		{
		    case "PT_UCHAR":
			vol.img = new float[vol.npix]; 
			break;
		    case "PT_SHORT":
			vol.img = new float[vol.npix]; 
			break;
		    case "PT_UINT16":
			vol.img = new float[vol.npix]; 
			break;
		    case "PT_UINT32":
			vol.img = new float[vol.npix]; 
			break;
		    case "PT_FLOAT":
			vol.img = new float[vol.npix]; 
			break;
		    case "PT_VF_FLOAT_INTERLEAVED":
			vol.img = new float[vol.npix * 3]; 
			break;
		    default:
			System.out.printf ("Unhandled type in volume_create().\n");
			System.exit (-1);
		}
	Arrays.fill(vol.img, 0.0f);
	return vol;
	}


	public void volume_calc_grad(Volume vout, Volume vref)
	{
        	int v;
        	int i_p, i, i_n, j_p, j, j_n, k_p, k, k_n; /* p is prev, n is next */
        	int gi, gj, gk;
        	int idx_p, idx_n;
        	float[] out_img;
        	float[] ref_img;
        	out_img = vout.img;
        	ref_img = vref.img;
        	v = 0;
        	for (k = 0; k < vref.dim[2]; k++)
        	{
		    	k_p = k - 1;
		    	k_n = k + 1;
		    	if (k == 0) 
		    	{
		        	k_p = 0;
		    	}
		    	if (k == vref.dim[2]-1) 
		    	{
		        	k_n = vref.dim[2]-1;
		    	}
		    	for (j = 0; j < vref.dim[1]; j++) 
		    	{
		        	j_p = j - 1;
		        	j_n = j + 1;
		        	if (j == 0) 
		        	{
		        	    j_p = 0;
		        	}
		        	if (j == vref.dim[1]-1) 
		        	{
		        	    j_n = vref.dim[1]-1;
		        	}
		        	for (i = 0; i < vref.dim[0]; i++, v++) 
		        	{
		       	    		i_p = i - 1;
		            		i_n = i + 1;
		            		if (i == 0) 
		            		{
		                		i_p = 0;
		            		}
		            		if (i == vref.dim[0]-1) 
		            		{
		                		i_n = vref.dim[0]-1;
		            		}
		            		gi = 3 * v + 0;
		            		gj = 3 * v + 1;
		            		gk = 3 * v + 2;
		            		idx_p = volume_index (vref.dim, i_p, j, k);
		            		idx_n = volume_index (vref.dim, i_n, j, k);
		            		out_img[gi] = (float)( (ref_img[idx_n] - ref_img[idx_p]) / 2.0 / vref.pix_spacing[0]);
		            		idx_p = volume_index (vref.dim, i, j_p, k);
		            		idx_n = volume_index (vref.dim, i, j_n, k);
		            		out_img[gj] = (float)( (ref_img[idx_n] - ref_img[idx_p]) / 2.0 / vref.pix_spacing[1]);
		            		idx_p = volume_index (vref.dim, i, j, k_p);
		            		idx_n = volume_index (vref.dim, i, j, k_n);
		            		out_img[gk] = (float)((ref_img[idx_n] - ref_img[idx_p]) / 2.0 / vref.pix_spacing[2]);
		        	}
		    	}
		}
        vout.img = out_img;
	}
}
```
