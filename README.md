# Introduction  
This repository complements a SkyServe case study found [here]() that aims to bring out differences in data footprint between a conventional cloud-first geospatial solutions and a hybrid edge-first cloud-hosted geospatial solution. The data used for the in this analysis is open source from the ESA Copernicus Sentinel-2 satellites.

# Data  
For both methods, the pre-processing (i.e. transformation from Raw to TOA/BOA products) is assumed to be identical. All raster data used here has undergone LZW compression, however if decompressed, the image size for all scenes is 460MB. Almost all EO operators today use LZW or similar performing lossless compression algorithms, and the same has been proven with satellite edge computers onboard satellites.

## Conventional Cloud-first Solutions  
This is to replicate the conventional cloud solution with images. In this scenario, there is no edge computing present. The data is imaged by the satellited and directly downlinked, hence even the pixels which are not usefull are downlinked. The location in the dataset is Chicago downtown and the image range is from [09 April 2024 - 29 April 2024]. The output data can be found [here](https://workdrive.zohopublic.in/external/b78df4780a28d02bb3f71381bd80ce78b6b336efd5680e9b93993cabdc5b28d2).  

|Sl No| Dataset Name | Size (MB) | 
| --- | --- | --- |
| 1. | image_20240409_norm_compressed.tif | 389 |
| 2. | image_20240414_norm_compressed.tif | 310 |
| 3. | image_20240419_norm_compressed.tif | 386 |
| 4. | image_20240424_norm_compressed.tif | 366 |
| 5. | image_20240429_norm_compressed.tif | 311 |

## Solution 1 - Edge Cloud Compress  
An average of 30% cloud cover is observed for any location on earth. In this scenario, there is a level 1 edge computing solution which is capable of masking the cloudy pixels and changing their values to 0 which increases the compression ratio for the image. This is a hybrid solution of edge computing plus cloud. The location in the dataset is Chicago downtown and the image range is from [09 April 2024 - 29 April 2024]. The output data can be found [here](https://workdrive.zohopublic.in/external/9b5a660f50a168297313a3404bfd62fba5b7d1dbf1ba4433ee6ef73de6be2add).    
 

|Sl No| Dataset Name | Cloud Cover | Original Size (MB) | Edge Computing Size (MB) |
| --- | --- | --- | --- | --- |
| 1. | image_20240409_masked_compressed.tif | 27.44% | 389 | 286 |
| 2. | image_20240414_masked_compressed.tif | 30.88% | 310 | 186 |
| 3. | image_20240419_masked_compressed.tif | 0.75% | 386 | 386 |
| 4. | image_20240424_masked_compressed.tif | 88.09% | 366 | 59.5 |
| 5. | image_20240429_masked_compressed.tif | 99.98% | 311 | 3.74 |

## Solution 2 - Feature Select
To derive value addition from the satellite images, additional processing is require which detects targetted features from the image. This provides a output which is a segmentation mask. Where the features are identified into classes and can be written. For this example we have a multi-class segmentation with the following classes {0: Cloud, 1: Water, 2: Forest, 3: Vegetation, 4: Urban}, the masks have a min of 0 and a max of 4. This is a hybrid solution of edge computing plus cloud. The location in the dataset is Chicago downtown and the image range is from [09 April 2024 - 29 April 2024]. The output data can be found [here](https://workdrive.zohopublic.in/external/150a10704d80f6295db564e04adca07f9b4dc424e4336c84844b1b987192b2ef).  


|Sl No| Dataset Name | Original Size (MB) | Edge Computing Size (MB) |
| --- | --- | --- | --- |
| 1. | clubbed_masks_20240409.tif | 389 | 7.33 |
| 2. | clubbed_masks_20240414.tif | 310 | 8 |
| 3. | clubbed_masks_20240419.tif | 386 | 9.68 |
| 4. | clubbed_masks_20240424.tif | 366 | 4 |
| 5. | clubbed_masks_20240429.tif | 311 | 1.1 |

## Solution 3 - Edge Vision
In this solution, object detection is performed where only the geocoded bounding boxes are downloaded. This significantly reduces the size. For this example we have a fire detection application where the output is a geoJSON with details of pixel where fire was detected. The location in the dataset is Patiala region in Punjab and the image range is from [16 October 2023 - 05 November 2023]. The output data can be found [here](https://workdrive.zohopublic.in/external/32d6bdb7f7310222f205e9a1938ae876d01a3dafe7985e301d4afb4f150bacec).    


|Sl No| Dataset Name | Number of Detections | Original Size (MB) | Edge Computing Size (MB) |
| --- | --- | --- | --- | --- |
| 1. | fire_detection_20231016.geojson | 26 | 389 | 0.005 |
| 2. | fire_detection_20231021.geojson | 341 | 310 | 0.062 |
| 3. | fire_detection_20231026.geojson | 526 | 386 | 0.095 |
| 4. | fire_detection_20231031.geojson | 376 | 366 | 0.068 |
| 5. | fire_detection_20231105.geojson | 313 | 311 | 0.057 |
