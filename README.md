# AlternativeBiometric ()

IMPLEMENTED BY AANCHAL AND TEANCY


This is a research paper implementation of A Secure Mobile Authentication Alternative to Biometrics

A-Secure-Mobile-Authentication-Alternative-to-Biometrics

ai.lock is a image-based authentication mechanism, alternative to biometrics. ai.lock uses deep neural networks, to extract feature images, PCA and LSH for fast and secure image matching. See our paper in ACSAC'17: "A Secure Mobile Authentication Alternative to Biometrics".

Datasets Nexus dataset Google Image dataset Toys dataset

Python Code Requirements: Tensorflow, Nearpy , h5py, scikit-image, numpy, sklearn

Computing Inception v3 activations for a dataset of images To create the required datasets for the experiments, take the following steps:
Use the Tensorflow official example code for computing the activations of a desired layer of Inception v3 network for all the images in ALOI, Google and Yfcc100m datasets. Note that, the parameters “BOTTLENECK_TENSOR_NAME” and “BOTTLENECK_TENSOR_SIZE” should be set according to the desired Inception layer ‘mixed_10/pool:0’ and 49152 for Mixed10_pool0 layer). For each image in the image dataset, the code will generate a text file that contains the activation of the specified layer of Inception v3 when the image is fed as the input to the network.
For each image dataset, use the aggregate_feature_vectors.ipynb notebook separately to aggregate the individual files generated in the previous step into a single h5py dataset. You need to set the “image_dir”, “out_file_name”, “google_images_flag” and “toys_images_flag” parameters accordingly.
For each image dataset, use the split_images.py to split images into 5 overlapping segments (for ai.lock multi segment experiments), and run step 1 and 2 to compute the datasets corresponding to activations of the Inception v3 for each segment of the images. Arrange the dataset that are generated from steps 1-3 into the following directory hierarchy: The datasets corresponding to the Mixed10/pool0 layer of Inception v3 go under “Mixed10_pool0” directories. For each layer, put the activations corresponding to the whole size images under the “full_size” directory, and the activations corresponding to each of the 5 image segments under “part_1” to “part_5” directories. The final directory structure for the datasets should look like the following. There should be 3 files under each subdirectory, that are, Nexus.h5, Toys.h5 and Google.h5.
Datasets


        full_size
        part_1
        part_2
        part_3
        part_4
        part_5
 
For each inception layer, run the split_datasets_to_test_train.py to split the embeddings in each dataset to test (holdout) and train sets. You need to set the parameter “layer_name” for Mixed10_pool0 layers.
Download the Nearpy package. Then, replace the last line of hash_vector method in hashes/permutations/randombinaryprojections.py (i.e. return [''.join(['1' if x > 0.0 else '0' for x in projection])]) to the following code: projection[projection > 0] = 1 projection[projection <= 0] = 0 return projection
Running Single/Multi Layer Multi Image experiments Use the code provided under Multi_image directory. To compute the best performing thresholds for binary classifications of images, and evaluate the performance of ai.lock on holdout set use multi_image.py.
